* **main.py:**

```python
from fastapi import FastAPI
from fastapi.middleware.cors import CORSMiddleware
from sqlmodel import SQLModel
from app.database import create_db_and_tables
from app.routers import auth, products, users, search, affiliates, categories

app = FastAPI(title="VestaAPI")

# CORS Setup
origins = ["http://localhost:3000", "https://your-vesta-domain.com"]
app.add_middleware(
    CORSMiddleware,
    allow_origins=origins,
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)

# Database Naming Convention
SQLModel.metadata.naming_convention = {
    "ix": "ix_%(column_0_label)s",
    "uq": "uq_%(table_name)s_%(column_0_name)s",
    "ck": "ck_%(table_name)s_%(constraint_name)s",
    "fk": "fk_%(table_name)s_%(column_0_name)s_%(referred_table_name)s",
    "pk": "pk_%(table_name)s"
}

@app.on_event("startup")
def on_startup():
    create_db_and_tables()

# Include Routers
app.include_router(auth.router)
app.include_router(products.router)
app.include_router(users.router)
app.include_router(search.router)
app.include_router(affiliates.router)
app.include_router(categories.router)

```

* **app/database.py**

```python
import os
from dotenv import load_dotenv
from sqlmodel import SQLModel, create_engine, Session
from app.models import User, Product, Comment, ProductLike

load_dotenv()

# Asegúrate de tener DATABASE_URL en tu archivo .env
sqlite_url = os.getenv("DATABASE_URL")

# El echo=True sirve para ver en consola todas las consultas SQL que se ejecutan (útil para aprender)
engine = create_engine(
    sqlite_url,
    echo=True,
    pool_pre_ping=True,  # Verifica si la conexión está viva antes de usarla
    connect_args={"connect_timeout": 10} # Da un poco más de tiempo para conectar
)

def create_db_and_tables():
    SQLModel.metadata.create_all(engine)

def get_session():
    with Session(engine) as session:
        yield session

```

## app/models/:

* ==> app/models/__init__.py <==
```python
from .interactions import ProductLike
from .users import User
from .products import Product, Comment
from .affiliates import AffiliateLink, ClickEvent
from .categories import Category

# Rebuild internal SQLModel relations
User.model_rebuild()
Product.model_rebuild()
Comment.model_rebuild()
AffiliateLink.model_rebuild()
Category.model_rebuild()

__all__ = ["User", "Product", "Comment", "ProductLike", "AffiliateLink", "ClickEvent", "Category"]
```

* ==> app/models/affiliates.py <==

```python
from datetime import datetime
from typing import Optional, List, TYPE_CHECKING
from sqlmodel import SQLModel, Field, Relationship

# Importación para el tipado sin causar círculos
if TYPE_CHECKING:
    from .products import Product
    from .users import User

class AffiliateLinkBase(SQLModel):
    platform_name: str
    url: str
    is_active: bool = Field(default=True)

class AffiliateLink(AffiliateLinkBase, table=True):
    id: Optional[int] = Field(default=None, primary_key=True)
    product_id: int = Field(foreign_key="product.id")

    # Relaciones
    product: "Product" = Relationship(back_populates="affiliate_links")
    clicks: List["ClickEvent"] = Relationship(back_populates="affiliate_link")

class ClickEvent(SQLModel, table=True):
    id: Optional[int] = Field(default=None, primary_key=True)
    link_id: int = Field(foreign_key="affiliatelink.id")
    user_id: Optional[int] = Field(default=None, foreign_key="user.id")
    referrer: Optional[str] = None
    created_at: datetime = Field(default_factory=datetime.utcnow)

    affiliate_link: AffiliateLink = Relationship(back_populates="clicks")
```

* ==> app/models/categories.py <==

```python
from typing import Optional, List, TYPE_CHECKING
from sqlmodel import SQLModel, Field, Relationship

if TYPE_CHECKING:
    from .products import Product

class Category(SQLModel, table=True):
    id: Optional[int] = Field(default=None, primary_key=True)
    name: str = Field(index=True, unique=True)
    description: Optional[str] = None
    slug: str = Field(unique=True) # Para URLs bonitas como /categories/laptops-gaming

    # Relación: Una categoría tiene muchos productos
    products: List["Product"] = Relationship(back_populates="category_rel")
```

* ==> app/models/interactions.py <==

```python
from sqlmodel import SQLModel, Field
from sqlalchemy import UniqueConstraint

class ProductLike(SQLModel, table=True):
    __table_args__ = (UniqueConstraint("user_id", "product_id", name="unique_user_product_like"),)
    user_id: int = Field(foreign_key="user.id", primary_key=True)
    product_id: int = Field(foreign_key="product.id", primary_key=True)

```


* ==> app/models/products.py <==

```python
from typing import Optional, List, TYPE_CHECKING
from sqlmodel import SQLModel, Field, Relationship
from datetime import datetime

# Importamos las clases de enlace y relación
from .interactions import ProductLike
from .affiliates import AffiliateLink
from .categories import Category

# Solo para tipado estático
if TYPE_CHECKING:
    from .users import User

class Product(SQLModel, table=True):
    id: Optional[int] = Field(default=None, primary_key=True)
    title: str
    description: str
    price: float
    image_url: Optional[str] = Field(default="https://via.placeholder.com/150")
    # Se eliminó el campo individual 'affiliate_link' porque ahora usamos la relación One-to-Many

    # Llave foránea y relación
    category_id: Optional[int] = Field(default=None, foreign_key="category.id")
    category_rel: Optional[Category] = Relationship(back_populates="products")

    created_at: datetime = Field(default_factory=datetime.utcnow)

    @property
    def likes_count(self) -> int:
        return len(self.favorited_by)

    # Relación con el dueño
    owner_id: int = Field(foreign_key="user.id")
    owner: Optional["User"] = Relationship(back_populates="products")

    # Relación con comentarios
    comments: List["Comment"] = Relationship(
        back_populates="product",
        sa_relationship_kwargs={"cascade": "all, delete-orphan"}
    )

    # Relación de Likes (Muchos a Muchos)
    favorited_by: List["User"] = Relationship(
        back_populates="liked_products",
        link_model=ProductLike,
    )

    # Esta es la relación que reemplaza al campo eliminado
    affiliate_links: List["AffiliateLink"] = Relationship(back_populates="product")


class Comment(SQLModel, table=True):
    id: Optional[int] = Field(default=None, primary_key=True)
    content: str
    created_at: datetime = Field(default_factory=datetime.utcnow)

    user_id: int = Field(foreign_key="user.id")
    product_id: int = Field(foreign_key="product.id")

    product: Product = Relationship(back_populates="comments")
    user: "User" = Relationship(back_populates="comments")
```

* ==> app/models/users.py <==

```python
from typing import Optional, List, TYPE_CHECKING
from sqlmodel import SQLModel, Field, Relationship
from .interactions import ProductLike

if TYPE_CHECKING:
    from .products import Product, Comment

class User(SQLModel, table=True):
    id: Optional[int] = Field(default=None, primary_key=True)
    username: str = Field(index=True, unique=True)
    email: str = Field(unique=True)
    hashed_password: str
    bio: Optional[str] = Field(default="Hello! I am a new developer on Vesta.")
    profile_pic: Optional[str] = Field(default="https://default-path.com/avatar.png")
    website: Optional[str] = None
    is_admin: bool = Field(default=False) # Adding the admin flag we discussed

    products: List["Product"] = Relationship(back_populates="owner")
    liked_products: List["Product"] = Relationship(
        back_populates="favorited_by",
        link_model=ProductLike
    )
    comments: List["Comment"] = Relationship(back_populates="user")
```

## app/schemas/:
* ==> app/schemas/affiliates.py <==

```python
from pydantic import BaseModel, field_validator
from typing import Optional
import bleach

class AffiliateLinkBase(BaseModel):
    platform_name: str
    url: str
    is_active: bool = True

    @field_validator("platform_name")
    @classmethod
    def sanitize_platform(cls, v):
        if v:
            return bleach.clean(v, tags=[], strip=True).strip()
        return v

class AffiliateLinkCreate(AffiliateLinkBase):
    product_id: int

class AffiliateLinkPublic(AffiliateLinkBase):
    id: int
```

* ==> app/schemas/categories.py <==

```python
# app/schemas/categories.py
from pydantic import BaseModel, field_validator
from typing import Optional
import bleach

class CategoryBase(BaseModel):
    name: str
    description: Optional[str] = None
    slug: str

    @field_validator("name", "description", "slug")
    @classmethod
    def sanitize_category(cls, v):
        if v:
            return bleach.clean(v, tags=[], strip=True).strip()
        return v

class CategoryCreate(CategoryBase):
    pass

class CategoryPublic(CategoryBase):
    id: int
```

* ==> app/schemas/products.py <==

```python
from pydantic import BaseModel, field_validator
from typing import Optional
import bleach

class ProductBase(BaseModel):
    title: str
    description: str
    price: float
    image_url: Optional[str] = "https://via.placeholder.com/150"
    category_id: int

    @field_validator("title", "description")
    @classmethod
    def sanitize_product_data(cls, v):
        if v:
            # Eliminamos cualquier intento de inyección de código
            return bleach.clean(v, tags=[], strip=True).strip()
        return v

    @field_validator("price")
    @classmethod
    def validate_price(cls, v):
        if v < 0:
            raise ValueError("Price cannot be negative")
        return v

class ProductCreate(ProductBase):
    pass

class ProductUpdate(BaseModel):
    title: Optional[str] = None
    description: Optional[str] = None
    price: Optional[float] = None
    image_url: Optional[str] = None
    category_id: Optional[int] = None

    @field_validator("title", "description")
    @classmethod
    def sanitize_update(cls, v):
        if v:
            return bleach.clean(v, tags=[], strip=True).strip()
        return v
```

*==> app/schemas/users.py <==

```python
from pydantic import BaseModel, EmailStr, field_validator
from typing import Optional, List
import bleach

class UserBase(BaseModel):
    username: str
    email: EmailStr
    bio: Optional[str] = None
    profile_pic: Optional[str] = None
    website: Optional[str] = None

    @field_validator("username", "bio")
    @classmethod
    def sanitize_text(cls, v, info):
        if v:
            # Quitamos HTML y espacios extra
            clean_v = bleach.clean(v, tags=[], strip=True).strip()
            return clean_v
        return v

class UserCreate(UserBase):
    password: str

class UserUpdate(BaseModel):
    bio: Optional[str] = None
    profile_pic: Optional[str] = None
    website: Optional[str] = None
    password: Optional[str] = None

    @field_validator("bio")
    @classmethod
    def sanitize_bio(cls, v):
        if v:
            return bleach.clean(v, tags=[], strip=True).strip()
        return v

class UserPublic(UserBase):
    id: int
    total_reputation: int
    products_count: int
    products: List = []

    class Config:
        from_attributes = True
```

## app/core/:

* ==> app/core/auth_utils.py <==

```python
from datetime import datetime, timedelta, timezone
from typing import Optional
import jose.jwt
from passlib.context import CryptContext
from .security_config import SECRET_KEY, ALGORITHM, ACCESS_TOKEN_EXPIRE_MINUTES

# Configuración para el hashing de contraseñas
pwd_context = CryptContext(schemes=["bcrypt"], deprecated="auto")

# --- Funciones de Contraseñas ---
def hash_password(password: str) -> str:
    return pwd_context.hash(password)

def verify_password(plain_password: str, hashed_password: str) -> bool:
    return pwd_context.verify(plain_password, hashed_password)

# --- Funciones de JWT (Hardened) ---
def create_access_token(data: dict, expires_delta: Optional[timedelta] = None):
    to_encode = data.copy()
    if expires_delta:
        expire = datetime.now(timezone.utc) + expires_delta
    else:
        expire = datetime.now(timezone.utc) + timedelta(minutes=ACCESS_TOKEN_EXPIRE_MINUTES)

    to_encode.update({"exp": expire})
    encoded_jwt = jose.jwt.encode(to_encode, SECRET_KEY, algorithm=ALGORITHM)
    return encoded_jwt
```

* ==> app/core/security.py <==

```python
from fastapi import Depends, HTTPException, status
from fastapi.security import OAuth2PasswordBearer
import jose.jwt
from sqlmodel import Session, select
from app.database import get_session
from app.models.users import User
from .security_config import SECRET_KEY, ALGORITHM

oauth2_scheme = OAuth2PasswordBearer(tokenUrl="auth/login")

def get_current_user(
    token: str = Depends(oauth2_scheme),
    session: Session = Depends(get_session)
) -> User:
    credentials_exception = HTTPException(
        status_code=status.HTTP_401_UNAUTHORIZED,
        detail="Could not validate credentials",
        headers={"WWW-Authenticate": "Bearer"},
    )
    try:
        payload = jose.jwt.decode(token, SECRET_KEY, algorithms=[ALGORITHM])
        username: str = payload.get("sub")
        if username is None:
            # Aquí faltaba la instrucción que causaba el error
            raise credentials_exception
    except jose.jwt.JWTError:
        raise credentials_exception

    user = session.exec(select(User).where(User.username == username)).first()

    if user is None:
        raise credentials_exception

    return user

def get_current_admin_user(current_user: User = Depends(get_current_user)):
    if not current_user.is_admin:
        raise HTTPException(
            status_code=status.HTTP_403_FORBIDDEN,
            detail="The user does not have enough privileges"
        )
    return current_user
```

* ==> app/core/security_config.py <==

```python
import os

# En producción, esto se lee de variables de entorno
# Por ahora, usamos valores por defecto para tu desarrollo en Termux
SECRET_KEY = os.getenv("SECRET_KEY", "70678c3c7e743a3d5f9922e967a5f6a9c1489e53697e68")
ALGORITHM = "HS256"
ACCESS_TOKEN_EXPIRE_MINUTES = 30
```


## app/routers/

* ==> app/routers/__init__.py <==

```python
from . import auth, products, users, search, affiliates, categories
```

* ==> app/routers/affiliates.py <==

```python
from fastapi import APIRouter, Depends, HTTPException, Request, status
from fastapi.responses import RedirectResponse
from sqlmodel import Session, select
from typing import Optional, List
import jose
from app.database import get_session
from app.models.affiliates import AffiliateLink, ClickEvent
from app.schemas.affiliates import AffiliateLinkCreate
from app.models.users import User
from app.models.products import Product
from app.core.security import ALGORITHM, SECRET_KEY, get_current_user

router = APIRouter(prefix="/affiliates", tags=["Affiliates"])

def get_optional_user_id(request: Request) -> Optional[int]:
    auth_header = request.headers.get("Authorization")
    if not auth_header or not auth_header.startswith("Bearer "):
        return None
    try:
        token = auth_header.split(" ")[1]
        payload = jose.jwt.decode(token, SECRET_KEY, algorithms=[ALGORITHM])
        username = payload.get("sub")
        # Buscamos el ID real basado en el username del token
        user = session.exec(select(User).where(User.username == username)).first()
        return user.id if user else None
    except Exception:
        return None

@router.post("", response_model=AffiliateLink, status_code=status.HTTP_201_CREATED)
def create_affiliate_link(
    data: AffiliateLinkCreate,
    session: Session = Depends(get_session),
    current_user: User = Depends(get_current_user)
):
    product = session.get(Product, data.product_id)
    if not product:
        raise HTTPException(status_code=status.HTTP_404_NOT_FOUND, detail="Product not found")

    if product.owner_id != current_user.id:
        raise HTTPException(status_code=status.HTTP_403_FORBIDDEN, detail="Permission denied")

    new_link = AffiliateLink(**data.model_dump())
    session.add(new_link)
    session.commit()
    session.refresh(new_link)
    return new_link

@router.get("/go/{link_id}")
def redirect_and_track(
    link_id: int,
    request: Request,
    session: Session = Depends(get_session)
):
    link = session.get(AffiliateLink, link_id)
    if not link or not link.is_active:
        raise HTTPException(status_code=status.HTTP_404_NOT_FOUND, detail="Invalid or inactive link")

    new_click = ClickEvent(
        link_id=link.id,
        user_id=get_optional_user_id(request),
        referrer=request.headers.get("referer")
    )
    session.add(new_click)
    session.commit()
    return RedirectResponse(url=link.url)

@router.get("/product/{product_id}", response_model=List[AffiliateLink])
def get_product_links(product_id: int, session: Session = Depends(get_session)):
    statement = select(AffiliateLink).where(
        AffiliateLink.product_id == product_id,
        AffiliateLink.is_active == True
    )
    return session.exec(statement).all()

@router.get("/analytics/{product_id}")
def get_product_analytics(
    product_id: int,
    session: Session = Depends(get_session),
    current_user: User = Depends(get_current_user)
):
    product = session.get(Product, product_id)
    if not product or product.owner_id != current_user.id:
        raise HTTPException(status_code=status.HTTP_403_FORBIDDEN, detail="Access denied")

    stats = [
        {
            "platform": link.platform_name,
            "total_clicks": len(link.clicks),
            "unique_users": len({c.user_id for c in link.clicks if c.user_id})
        }
        for link in product.affiliate_links
    ]
    return {"product_title": product.title, "analytics": stats}
```

* ==> app/routers/auth.py <==

```python
from fastapi import APIRouter, Depends, HTTPException, status
from fastapi.security import OAuth2PasswordRequestForm
from sqlmodel import Session, select
from app.database import get_session
from app.models.users import User
from app.schemas.users import UserCreate # Schema imported from new location
from app.core.auth_utils import hash_password, verify_password, create_access_token

router = APIRouter(prefix="/auth", tags=["Auth"])

@router.post("/register", status_code=status.HTTP_201_CREATED)
def register(user_data: UserCreate, session: Session = Depends(get_session)):
    # Check for existing user by email or username
    existing_user = session.exec(
        select(User).where((User.email == user_data.email) | (User.username == user_data.username))
    ).first()

    if existing_user:
        raise HTTPException(
            status_code=status.HTTP_400_BAD_REQUEST,
            detail="Username or email already registered"
        )

    new_user = User(
        username=user_data.username,
        email=user_data.email,
        hashed_password=hash_password(user_data.password)
    )
    session.add(new_user)
    session.commit()
    session.refresh(new_user)
    return {"message": "User created successfully", "id": new_user.id}

@router.post("/login")
def login(
    form_data: OAuth2PasswordRequestForm = Depends(),
    session: Session = Depends(get_session)
):
    user = session.exec(select(User).where(User.username == form_data.username)).first()
    if not user or not verify_password(form_data.password, user.hashed_password):
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="Incorrect username or password",
            headers={"WWW-Authenticate": "Bearer"},
        )

    access_token = create_access_token(data={"sub": user.username})
    return {"access_token": access_token, "token_type": "bearer"}
```

* ==> app/routers/categories.py <==

```python
from fastapi import APIRouter, Depends, HTTPException, status
from sqlmodel import Session, select
from typing import List
from app.database import get_session
from app.models.categories import Category
from app.models.users import User
from app.core.security import get_current_admin_user
from sqlalchemy.exc import IntegrityError
import re # Para validar slugs manualmente si fuera necesario

router = APIRouter(prefix="/categories", tags=["Categories"])

@router.get("", response_model=List[Category])
def get_categories(session: Session = Depends(get_session)):
    """Public endpoint to list all categories."""
    return session.exec(select(Category)).all()

@router.post("", response_model=Category, status_code=status.HTTP_201_CREATED)
def create_category(
    category_in: CategoryCreate,
    session: Session = Depends(get_session),
    current_admin: User = Depends(get_current_admin_user)
):
    """
    Protected endpoint: Only users with is_admin=True can create categories.
    """
    new_category = Category(**category_in.model_dump())
    new_category.slug = new_category.slug.lower().replace(" ", "-")

    # Blindaje 2: Validación de formato de slug (Solo letras, números y guiones)
    if not re.match(r'^[a-z0-9-]+$', category.slug):
        raise HTTPException(
            status_code=status.HTTP_400_BAD_REQUEST,
            detail="Slug must contain only lowercase letters, numbers, and hyphens."
        )

    try:
        session.add(category)
        session.commit()
        session.refresh(category)
        return category
    except IntegrityError:
        session.rollback()
        # Blindaje 3: Error informativo para el cliente
        raise HTTPException(
            status_code=status.HTTP_400_BAD_REQUEST,
            detail=f"Category with slug '{category.slug}' or name '{category.name}' already exists."
        )


==> app/routers/products.py <==
from fastapi import APIRouter, Depends, HTTPException, Query, status
from sqlmodel import Session, select
from typing import List
from app.database import get_session
from app.models.products import Product
from app.models.users import User
from app.schemas.products import ProductCreate, ProductUpdate # New Imports
from app.core.security import get_current_user
from sqlalchemy.exc import IntegrityError

router = APIRouter(prefix="/products", tags=["Products"], redirect_slashes=False)

@router.get("", response_model=List[Product])
def get_products(
    offset: int = 0,
    limit: int = Query(default=100, le=100),
    session: Session = Depends(get_session)
):
    return session.exec(select(Product).offset(offset).limit(limit)).all()

@router.post("", response_model=Product, status_code=status.HTTP_201_CREATED)
def create_product(
    product_in: ProductCreate, # Using sanitized schema
    session: Session = Depends(get_session),
    current_user: User = Depends(get_current_user)
):
    # Convert schema to DB model and attach owner
    new_product = Product(**product_in.model_dump(), owner_id=current_user.id)

    session.add(new_product)
    try:
        session.commit()
    except IntegrityError:
        session.rollback()
        raise HTTPException(
            status_code=status.HTTP_400_BAD_REQUEST,
            detail=f"Invalid category_id: {product_in.category_id} does not exist."
        )

    session.refresh(new_product)
    return new_product

@router.delete("/{product_id}")
def delete_product(
    product_id: int,
    session: Session = Depends(get_session),
    current_user: User = Depends(get_current_user)
):
    product = session.get(Product, product_id)
    if not product:
        raise HTTPException(status_code=status.HTTP_404_NOT_FOUND, detail="Product not found")

    if product.owner_id != current_user.id:
        raise HTTPException(
            status_code=status.HTTP_403_FORBIDDEN,
            detail="Not enough permissions to delete this product"
        )

    session.delete(product)
    session.commit()
    return {"message": "Product deleted successfully"}
```

* ==> app/routers/search.py <==

```python
from fastapi import APIRouter, Depends, Query # Added Query
from sqlmodel import Session, select, or_
from typing import List, Optional
from app.database import get_session
from app.models.products import Product # Specific import

router = APIRouter(prefix="/search", tags=["Search"])

@router.get("", response_model=List[Product])
def search_products(
    q: Optional[str] = None,
    min_price: Optional[float] = None,
    max_price: Optional[float] = None,
    sort_by: str = "newest", # Options: newest, lowest_price, highest_price
    limit: int = Query(default=20, le=50),
    session: Session = Depends(get_session)
):
    statement = select(Product)

    # 1. Full-text search (Title or Description)
    if q:
        statement = statement.where(
            or_(
                Product.title.icontains(q),
                Product.description.icontains(q)
            )
        )

    # 2. Price Range Filtering
    if min_price is not None:
        statement = statement.where(Product.price >= min_price)
    if max_price is not None:
        statement = statement.where(Product.price <= max_price)

    # 3. Sorting Logic
    if sort_by == "lowest_price":
        statement = statement.order_by(Product.price.asc())
    elif sort_by == "highest_price":
        statement = statement.order_by(Product.price.desc())
    else:
        # Default: Newest first
        statement = statement.order_by(Product.created_at.desc())

    results = session.exec(statement.limit(limit)).all()
    return results
```

* ==> app/routers/users.py <==

```py
from fastapi import APIRouter, Depends, HTTPException, status
from sqlmodel import Session, select
from typing import List
from app.database import get_session
from app.models.users import User
from app.models.products import Product
from app.schemas.users import UserPublic, UserUpdate
from app.core.security import get_current_user
from sqlalchemy.orm import selectinload

router = APIRouter(prefix="/users", tags=["Users"])

# ... (get_my_products remains same) ...

@router.get("/me", response_model=User)
def get_my_profile(current_user: User = Depends(get_current_user)):
    return current_user

@router.put("/me", response_model=UserPublic)
def update_my_profile(
    user_data: UserUpdate, # Sanitization happens here automatically
    session: Session = Depends(get_session),
    current_user: User = Depends(get_current_user)
):
    update_dict = user_data.model_dump(exclude_unset=True)

    for key, value in update_dict.items():
        # Si incluiste password en UserUpdate, hay que hashearla antes de guardarla
        if key == "password" and value:
            from app.core.auth_utils import hash_password
            value = hash_password(value)
        setattr(current_user, key, value)

    session.add(current_user)
    session.commit()
    session.refresh(current_user)

    # Cálculo dinámico para la respuesta UserPublic
    reputation = sum(len(p.favorited_by) for p in current_user.products)
                                                return {
        **current_user.model_dump(),                "total_reputation": reputation,
        "products_count": len(current_user.products),
        "products": current_user.products       }
                                            # ... (get_user_profile remains same) ...
```

* **migrations/env.py:**

```python
import sys                                  from os.path import abspath, dirname

sys.path.insert(0, abspath(dirname(dirname(__file__))))

import os
from dotenv import load_dotenv
from logging.config import fileConfig
from sqlalchemy import engine_from_config
from sqlalchemy import pool

from alembic import context
from sqlmodel import SQLModel
from app.models.users import User
from app.models.products import Product
from app.models.categories import Category
from app.models.affiliates import AffiliateLink, ClickEvent

load_dotenv()

config = context.config

if os.getenv("DATABASE_URL"):
    config.set_main_option("sqlalchemy.url", os.getenv("DATABASE_URL"))

if config.config_file_name is not None:
    fileConfig(config.config_file_name)

# add your model's MetaData object here
# for 'autogenerate' support
# from myapp import mymodel
# target_metadata = mymodel.Base.metadata
target_metadata = SQLModel.metadata

# other values from the config, defined by the needs of env.py,
# can be acquired:
# my_important_option = config.get_main_option("my_important_option")
# ... etc.


def run_migrations_offline() -> None:
    """Run migrations in 'offline' mode.

    This configures the context with just a URL
    and not an Engine, though an Engine is acceptable
    here as well.  By skipping the Engine creation
    we don't even need a DBAPI to be available.
                                                Calls to context.execute() here emit the given string to the                            script output.
                                                """
    url = config.get_main_option("sqlalchemy.url")
    context.configure(                              url=url,
        target_metadata=target_metadata,            literal_binds=True,
        dialect_opts={"paramstyle": "named"},
    )                                       
    with context.begin_transaction():               context.run_migrations()
                                            
def run_migrations_online() -> None:            """Run migrations in 'online' mode.
                                                In this scenario we need to create an Engine                                            and associate a connection with the context.                                        
    """                                         connectable = engine_from_config(               config.get_section(config.config_ini_section, {}),                                      prefix="sqlalchemy.",
        poolclass=pool.NullPool,
    )

    with connectable.connect() as connection:
        context.configure(                              connection=connection, target_metadata=target_metadata                              )
                                                    with context.begin_transaction():
            context.run_migrations()        
                                            if context.is_offline_mode():
    run_migrations_offline()                else:
    run_migrations_online()
```

## migrations/versions/:

* ==> migrations/versions/b45293c9d002_remove_old_affiliate_field.py <==

```python
"""remove_old_affiliate_field               
Revision ID: b45293c9d002                   Revises: eeac6ab839e0
Create Date: 2026-02-02 09:27:14.484427     
"""                                         from typing import Sequence, Union
                                            from alembic import op
import sqlalchemy as sa                     import sqlmodel
                                            
# revision identifiers, used by Alembic.    revision: str = 'b45293c9d002'
down_revision: Union[str, Sequence[str], None] = 'eeac6ab839e0'
branch_labels: Union[str, Sequence[str], None] = None
depends_on: Union[str, Sequence[str], None] = None

def upgrade() -> None:
    """Upgrade schema."""
    # ### commands auto generated by Alembic - please adjust! ###
    op.drop_column('product', 'affiliate_link')
    #op.create_unique_constraint('unique_user_product_like', 'productlike', ['user_id', 'product_id'])                                  # ### end Alembic commands ###
                                            
def downgrade() -> None:                        """Downgrade schema."""
    # ### commands auto generated by Alembic - please adjust! ###
    #op.drop_constraint('unique_user_product_like', 'productlike', type_='unique')
    op.add_column('product', sa.Column('affiliate_link', sa.VARCHAR(), autoincrement=False, nullable=True))                             # ### end Alembic commands ###
```

* ==> migrations/versions/eeac6ab839e0_setup_database.py <==

```python
"""setup_database
                                            Revision ID: eeac6ab839e0
Revises:                                    Create Date: 2026-02-02 07:56:59.871758
                                            """
import sqlmodel                             from typing import Sequence, Union
                                            from alembic import op
import sqlalchemy as sa                     
                                            # revision identifiers, used by Alembic.
revision: str = 'eeac6ab839e0'              down_revision: Union[str, Sequence[str], None] = None                                   branch_labels: Union[str, Sequence[str], None] = None                                   depends_on: Union[str, Sequence[str], None] = None                                      

def upgrade() -> None:
    """Upgrade schema."""
    # ### commands auto generated by Alembic - please adjust! ###
    op.create_table('category',
    sa.Column('id', sa.Integer(), nullable=False),                                          sa.Column('name', sqlmodel.sql.sqltypes.AutoString(), nullable=False),
    sa.Column('description', sqlmodel.sql.sqltypes.AutoString(), nullable=True),
    sa.Column('slug', sqlmodel.sql.sqltypes.AutoString(), nullable=False),                  sa.PrimaryKeyConstraint('id'),
    sa.UniqueConstraint('slug')                 )
    op.create_index(op.f('ix_category_name'), 'category', ['name'], unique=True)
    op.create_table('user',                     sa.Column('id', sa.Integer(), nullable=False),                                          sa.Column('username', sqlmodel.sql.sqltypes.AutoString(), nullable=False),              sa.Column('email', sqlmodel.sql.sqltypes.AutoString(), nullable=False),                 sa.Column('hashed_password', sqlmodel.sql.sqltypes.AutoString(), nullable=False),       sa.Column('bio', sqlmodel.sql.sqltypes.AutoString(), nullable=True),                    sa.Column('profile_pic', sqlmodel.sql.sqltypes.AutoString(), nullable=True),            sa.Column('website', sqlmodel.sql.sqltypes.AutoString(), nullable=True),                sa.Column('is_admin', sa.Boolean(), nullable=False),                                    sa.PrimaryKeyConstraint('id'),
    sa.UniqueConstraint('email')                )
    op.create_index(op.f('ix_user_username'), 'user', ['username'], unique=True)
    op.create_table('product',
    sa.Column('id', sa.Integer(), nullable=False),
    sa.Column('title', sqlmodel.sql.sqltypes.AutoString(), nullable=False),
    sa.Column('description', sqlmodel.sql.sqltypes.AutoString(), nullable=False),
    sa.Column('price', sa.Float(), nullable=False),                                         sa.Column('image_url', sqlmodel.sql.sqltypes.AutoString(), nullable=True),
    sa.Column('affiliate_link', sqlmodel.sql.sqltypes.AutoString(), nullable=True),
    sa.Column('category_id', sa.Integer(), nullable=True),
    sa.Column('created_at', sa.DateTime(), nullable=False),
    sa.Column('owner_id', sa.Integer(), nullable=False),
    sa.ForeignKeyConstraint(['category_id'], ['category.id'], ),
    sa.ForeignKeyConstraint(['owner_id'], ['user.id'], ),                                   sa.PrimaryKeyConstraint('id')
    )
    op.create_table('affiliatelink',            sa.Column('platform_name', sqlmodel.sql.sqltypes.AutoString(), nullable=False),
    sa.Column('url', sqlmodel.sql.sqltypes.AutoString(), nullable=False),                   sa.Column('is_active', sa.Boolean(), nullable=False),                                   sa.Column('id', sa.Integer(), nullable=False),
    sa.Column('product_id', sa.Integer(), nullable=False),                                  sa.ForeignKeyConstraint(['product_id'], ['product.id'], ),
    sa.PrimaryKeyConstraint('id')
    )                                           op.create_table('comment',                  sa.Column('id', sa.Integer(), nullable=False),
    sa.Column('content', sqlmodel.sql.sqltypes.AutoString(), nullable=False),
    sa.Column('created_at', sa.DateTime(), nullable=False),                                 sa.Column('user_id', sa.Integer(), nullable=False),
    sa.Column('product_id', sa.Integer(), nullable=False),                                  sa.ForeignKeyConstraint(['product_id'], ['product.id'], ),
    sa.ForeignKeyConstraint(['user_id'], ['user.id'], ),                                    sa.PrimaryKeyConstraint('id')               )                                           op.create_table('productlike',
    sa.Column('user_id', sa.Integer(), nullable=False),
    sa.Column('product_id', sa.Integer(), nullable=False),                                  sa.ForeignKeyConstraint(['product_id'], ['product.id'], ),                              sa.ForeignKeyConstraint(['user_id'], ['user.id'], ),
    sa.PrimaryKeyConstraint('user_id', 'product_id'),                                       sa.UniqueConstraint('user_id', 'product_id', name='unique_user_product_like')           )
    op.create_table('clickevent',
    sa.Column('id', sa.Integer(), nullable=False),
    sa.Column('link_id', sa.Integer(), nullable=False),                                     sa.Column('user_id', sa.Integer(), nullable=True),
    sa.Column('referrer', sqlmodel.sql.sqltypes.AutoString(), nullable=True),               sa.Column('created_at', sa.DateTime(), nullable=False),                                 sa.ForeignKeyConstraint(['link_id'], ['affiliatelink.id'], ),
    sa.ForeignKeyConstraint(['user_id'], ['user.id'], ),                                    sa.PrimaryKeyConstraint('id')               )
    # ### end Alembic commands ###


def downgrade() -> None:                        """Downgrade schema."""                     # ### commands auto generated by Alembic - please adjust! ###
    op.drop_table('clickevent')
    op.drop_table('productlike')                op.drop_table('comment')                    op.drop_table('affiliatelink')              op.drop_table('product')                    op.drop_index(op.f('ix_user_username'), table_name='user')
    op.drop_table('user')
    op.drop_index(op.f('ix_category_name'), table_name='category')                          op.drop_table('category')                   # ### end Alembic commands ###
```



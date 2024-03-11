# DB-Assignment

1. Explain the relationship between the "Product" and "Product_Category" entities from the above diagram?

Answer -  The "Product" and "Product_Category" entities have a relationship defined by the "category_id" column in the "Product" table. This "category_id" column is an integer and serves as a foreign key, referencing the "id" column in the "Product_Category" table. This indicates that each product is associated with one product category, while each product category can have multiple products associated with it. In other words, the relationship between "Product" and "Product_Category" is a one-to-many relationship, with "Product_Category" being the "one" side and "Product" being the "many" side.

2.How could you ensure that each product in the "Product" table has a valid category assigned to it?

Answer - Each product in the "Product" table has a valid category assigned to it, you can implement a constraint in the database schema called a foreign key constraint. This constraint would enforce that the "category_id" column in the "Product" table should always reference an existing "id" in the "Product_Category" table.

3. Create schema in any Database script or any ORM (Object Relational Mapping)?

Answer -  This will be inDatabase script

CREATE TABLE product_category (
  id SERIAL PRIMARY KEY,
  name VARCHAR(255) NOT NULL,
  desc TEXT,
  created_at TIMESTAMP DEFAULT NOW(),
  modified_at TIMESTAMP DEFAULT NOW(),
  deleted_at TIMESTAMP
);

CREATE TABLE product_inventory (
  id SERIAL PRIMARY KEY,
  SKU VARCHAR(255) NOT NULL,
  quantity INT NOT NULL,
  category_id INT REFERENCES product_category(id),
  created_at TIMESTAMP DEFAULT NOW(),
  inventory_id INT,
  modified_at TIMESTAMP DEFAULT NOW(),
  price DECIMAL(10, 2),
  deleted_at TIMESTAMP,
  discount_id INT REFERENCES discount(id),
  created_at TIMESTAMP DEFAULT NOW(),
  discount MODIFY TIMESTAMP DEFAULT NOW()
);

CREATE TABLE product (
  id SERIAL PRIMARY KEY,
  name VARCHAR(255) NOT NULL,
  category_id INT REFERENCES product_category(id),
  created_at TIMESTAMP DEFAULT NOW(),
  deleted_at TIMESTAMP
);

CREATE TABLE discount (
  id SERIAL PRIMARY KEY,
  name VARCHAR(255) NOT NULL,
  desc TEXT,
  discount_percent DECIMAL(10, 2) NOT NULL,
  active BOOLEAN NOT NULL,
  created_at TIMESTAMP DEFAULT NOW(),
  modified_at TIMESTAMP DEFAULT NOW(),
  deleted_at TIMESTAMP
);

This will be in ORM  using python 

from sqlalchemy import Column, Integer, String, Text, DateTime, Boolean, Numeric, ForeignKey
from sqlalchemy.orm import relationship
from sqlalchemy.ext.declarative import declarative_base

Base = declarative_base()

class ProductCategory(Base):
    __tablename__ = 'product_category'
    id = Column(Integer, primary_key=True)
    name = Column(String(255), nullable=False)
    desc = Column(Text)
    created_at = Column(DateTime, default=datetime.datetime.utcnow)
    modified_at = Column(DateTime, default=datetime.datetime.utcnow)
    deleted_at = Column(DateTime)
    products = relationship("Product", back_populates="category")

class ProductInventory(Base):
    __tablename__ = 'product_inventory'
    id = Column(Integer, primary_key=True)
    SKU = Column(String(255), nullable=False)
    quantity = Column(Integer, nullable=False)
    category_id = Column(Integer, ForeignKey('product_category.id'))
    created_at = Column(DateTime, default=datetime.datetime.utcnow)
    inventory_id = Column(Integer)
    modified_at = Column(DateTime, default=datetime.datetime.utcnow)
    price = Column(Numeric(10, 2))
    deleted_at = Column(DateTime)
    discount_id = Column(Integer, ForeignKey('discount.id'))
    created_at = Column(DateTime, default=datetime.datetime.utcnow)
    discount = Column(DateTime, default=datetime.datetime.utcnow)

class Product(Base):
    __tablename__ = 'product'
    id = Column(Integer, primary_key=True)
    name = Column(String(255), nullable=False)
    category_id = Column(Integer, ForeignKey('product_category.id'))
    created_at = Column(DateTime, default=datetime.datetime.utcnow)
    deleted_at = Column(DateTime)
    category = relationship("ProductCategory", back_populates="products")

class Discount(Base):
    __tablename__ = 'discount'
    id = Column(Integer, primary_key=True)
    name = Column(String(255), nullable=


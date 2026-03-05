CREATE DATABASE IF NOT EXISTS lagro_bistro
  CHARACTER SET utf8mb4
  COLLATE utf8mb4_unicode_ci;

USE lagro_bistro;

CREATE TABLE IF NOT EXISTS users (
  id              INT           NOT NULL AUTO_INCREMENT,
  username        VARCHAR(50)   NOT NULL UNIQUE,
  password        VARCHAR(255)  NOT NULL,
  role            ENUM('customer','moderator','admin') NOT NULL DEFAULT 'customer',
  lrn             VARCHAR(50),
  grade           VARCHAR(50),
  section         VARCHAR(50),
  status          ENUM('active','banned') NOT NULL DEFAULT 'active',
  created_at      TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (id)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

CREATE TABLE IF NOT EXISTS products (
  id              INT           NOT NULL AUTO_INCREMENT,
  name            VARCHAR(100)  NOT NULL,
  price           DECIMAL(10,2) NOT NULL,
  qty             INT           NOT NULL DEFAULT 0,
  image           VARCHAR(255),
  featured        TINYINT(1)    DEFAULT 0,
  category        VARCHAR(100),
  description     TEXT,
  created_at      TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (id)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

CREATE TABLE IF NOT EXISTS orders (
  id              INT           NOT NULL AUTO_INCREMENT,
  username        VARCHAR(50)   NOT NULL,
  items           JSON,
  total           DECIMAL(10,2) NOT NULL,
  payment_method  VARCHAR(50),
  notes           TEXT,
  status          VARCHAR(50)   DEFAULT 'Processing',
  created_at      TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (id),
  FOREIGN KEY (username) REFERENCES users(username) ON DELETE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

INSERT INTO users (username, password, role, lrn, grade, section, status) VALUES
('admin', '5f4dcc3b5aa765d61d8327deb882cf99', 'admin', NULL, NULL, NULL, 'active'),
('demouser', '5f4dcc3b5aa765d61d8327deb882cf99', 'customer', '123456789012', 'Grade 10', 'A', 'active');

INSERT INTO products (name, price, qty, image, featured, category, description) VALUES
('Classic Coffee', 45.00, 15, 'assets/img/product1.png', 1, 'Coffee', 'Freshly brewed classic coffee'),
('Ham & Cheese Sandwich', 95.00, 8, 'assets/img/product2.png', 0, 'Sandwich', 'Delicious ham and cheese sandwich'),
('Chocolate Cake Slice', 75.00, 3, 'assets/img/product3.png', 1, 'Dessert', 'Rich chocolate cake slice'),
('Iced Tea', 40.00, 20, 'assets/img/product4.png', 0, 'Beverage', 'Refreshing iced tea'),
('Croissant', 55.00, 12, 'assets/img/product5.png', 1, 'Pastry', 'Buttery croissant'),
('Carbonara Pasta', 120.00, 5, 'assets/img/product6.png', 0, 'Pasta', 'Creamy carbonara pasta'),
('Fried Chicken', 85.00, 10, 'assets/img/product7.png', 0, 'Main Course', 'Crispy fried chicken'),
('Cheesecake', 90.00, 4, 'assets/img/product8.png', 1, 'Dessert', 'Creamy New York cheesecake');

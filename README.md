# BLOG-POST-V3: Full-Stack Blog Website with User Authentication & Comments

A production-ready Flask blog application featuring user authentication, admin-controlled blog post management, and a commenting system.

## 📚 Key Learnings (Day 69)

### 1. **Flask Web Framework & Application Setup**
- Initializing Flask app with secret key configuration
- Setting up Flask blueprints and routing
- Integrating multiple Flask extensions (Bootstrap, CKEditor, etc.)
- Debug mode configuration for development

### 2. **User Authentication & Security**
- **Password Hashing**: Using `werkzeug.security` (pbkdf2:sha256) to securely hash passwords with salt
- **User Authentication**: Implementing Flask-Login with custom `UserMixin` models
- **Session Management**: Managing user login/logout flows with proper redirects
- **Access Control**: Role-based authorization using admin decorator patterns
- **Custom Property**: Implementing `is_admin` property to identify admin users (user.id == 1)

### 3. **Database Architecture with SQLAlchemy ORM**
- **Modern SQLAlchemy 2.0 Syntax**: Using `Mapped` types and type hints for clean, type-safe model definitions
- **Declarative Base**: Custom `DeclarativeBase` class for proper SQLite integration
- **Relationships**: 
  - One-to-Many: User → BlogPost (authors write multiple posts)
  - One-to-Many: User → Comment (users leave multiple comments)
  - One-to-Many: BlogPost → Comment (posts have multiple comments)
- **Foreign Keys**: Properly defining relationships with ForeignKey constraints
- **Back-populates**: Bidirectional relationships for easy navigation

### 4. **Data Models**
```
User (users table)
  ├── id (Primary Key)
  ├── username (unique)
  ├── email (unique)
  ├── password (hashed)
  ├── blog_posts (relationship)
  └── comments (relationship)

BlogPost (blog_posts table)
  ├── id (Primary Key)
  ├── title (unique)
  ├── subtitle
  ├── date
  ├── body (rich text)
  ├── img_url
  ├── author_id (FK → User)
  ├── author (relationship)
  └── comments (relationship)

Comment (comments table)
  ├── id (Primary Key)
  ├── text
  ├── author_id (FK → User)
  ├── author (relationship)
  ├── post_id (FK → BlogPost)
  └── post (relationship)
```

### 5. **Form Validation & CKEditor Integration**
- WTForms with Flask-WTF for server-side validation
- CKEditor integration for rich text editing of blog posts
- Custom form classes: `CreatePostForm`, `RegisterForm`, `LoginForm`, `CommentForm`
- CSRF protection through Flask-Bootstrap5

### 6. **Database Operations & Error Handling**
- Session management with SQLAlchemy (`db.session.add()`, `db.session.commit()`)
- IntegrityError handling for duplicate username/email detection
- Using `db.get_or_404()` for safe database queries with automatic 404 handling
- Session rollback on integrity errors
- Automatic database creation with `db.create_all()`

### 7. **Authentication Routes**
- **Registration**: User signup with duplicate username/email validation
- **Login**: Supporting both username and email as login identifiers
- **Logout**: Clean session termination
- **User Loader**: Flask-Login user_loader callback for session persistence

### 8. **Admin-Only Features**
- Decorator pattern (`@admin_only`) for protecting admin routes
- Create new blog posts
- Edit existing posts (preserving author and date)
- Delete posts
- Returns 403 Forbidden for unauthorized access

### 9. **Commenting System**
- Authenticated users only can leave comments
- Comments linked to both User and BlogPost
- Comments displayed on individual post pages
- Non-authenticated users redirected to login when attempting to comment

### 10. **Flask Gravatar Integration**
- Dynamic user profile pictures based on email addresses
- Professional-looking avatar system without storing images

### 11. **Blog Management Routes**
- **GET /**: List all blog posts
- **GET /post/<id>**: View single post with comments
- **GET/POST /new-post**: Create new post (admin only)
- **GET/POST /edit-post/<id>**: Edit existing post (admin only)
- **GET /delete/<id>**: Delete post (admin only)
- **GET /about**: About page
- **GET /contact**: Contact page

### 12. **Best Practices Implemented**
- Separation of concerns with forms in separate `forms.py` file
- Modular database operations with helper functions (`_add()`, `_get_user_by_username()`, etc.)
- Proper use of Flask decorators for route protection
- User feedback with flash messages (success, warning, danger)
- Type hints throughout the codebase
- DRY principle with reusable edit form for both create and edit operations

### 13. **Python Dependency Management with uv** ⭐
- **uv** is a modern Python package manager that's significantly faster than pip
- Uses `uv.lock` file for reproducible, consistent dependency resolution
- Lock files ensure all developers and CI/CD environments use identical package versions
- Faster sync times with `uv sync` compared to `pip install`
- Best practice: commit `uv.lock` to version control for dependency tracking

## 🛠️ Tech Stack
- **Backend**: Flask, SQLAlchemy ORM
- **Database**: SQLite
- **Authentication**: Flask-Login, Werkzeug Security
- **Forms**: WTForms, Flask-WTF
- **Rich Text Editor**: CKEditor
- **Frontend Framework**: Bootstrap 5
- **Utilities**: Flask-Gravatar, Flask-CKEditor
- **Package Manager**: uv (with pip fallback)

## 📦 Dependencies
See `requirements.txt` for all dependencies and `uv.lock` for pinned versions. Key packages:
- Flask
- Flask-SQLAlchemy
- Flask-Login
- Flask-Bootstrap
- Flask-CKEditor
- WTForms
- Flask-Gravatar

## 🚀 Running the Application

### Option 1: Using uv (Recommended - Faster & Reproducible) ⭐
```bash
# Install uv if you haven't already
# macOS/Linux:
curl -LsSf https://astral.sh/uv/install.sh | sh

# Windows (via pip):
pip install uv

# Install dependencies using uv.lock for reproducible builds
uv sync

# Run the app
python main.py

# Access at http://localhost:5002
```

### Option 2: Using pip
```bash
# Install dependencies
pip3 install -r requirements.txt

# Run the app
python main.py

# Access at http://localhost:5002
```

## 🎓 Advanced Concepts Covered
- Object-Relational Mapping (ORM) patterns
- Authentication & Authorization
- Password security and hashing
- Database normalization
- One-to-Many relationships
- Flask extension patterns
- Route protection with decorators
- Form validation and CSRF protection
- Modern Python dependency management with lock files

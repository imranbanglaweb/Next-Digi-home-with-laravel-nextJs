# Quick Start Guide - Dynamic Content Integration

## 🚀 Get Up and Running in 5 Minutes

### Prerequisites
- Node.js 18+
- PHP 8.1+
- MySQL 8.0+
- Laravel 11+

## Step 1: Backend Setup (3 minutes)

### 1.1 Run Migrations
```bash
cd backend
php artisan migrate
```

### 1.2 Seed Initial Data
```bash
php artisan db:seed
# or specific seeder
php artisan db:seed --class=ContentSeeder
```

### 1.3 Start Backend Server
```bash
php artisan serve
# Backend now running at http://localhost:8000
```

### 1.4 Test API
```bash
# In another terminal, test the endpoint
curl http://localhost:8000/api/content/home

# Should see response with success: true
```

## Step 2: Frontend Setup (2 minutes)

### 2.1 Navigate to Frontend
```bash
cd frontend
```

### 2.2 Check Environment Variables
Verify `.env.local` exists with:
```
NEXT_PUBLIC_API_URL=http://localhost:8000
NEXT_PUBLIC_API_BASE_PATH=/api
```

### 2.3 Install Dependencies (if needed)
```bash
npm install
```

### 2.4 Start Frontend Dev Server
```bash
npm run dev
# Frontend now running at http://localhost:3000
```

## Step 3: Verify Integration

### 3.1 Open Browser
Visit http://localhost:3000

### 3.2 Check Each Page

- **Home**: http://localhost:3000
  - ✅ Hero slider loads
  - ✅ Stats display
  - ✅ Features show

- **About**: http://localhost:3000/about
  - ✅ Mission/Vision load
  - ✅ Team members display
  - ✅ Stats show

- **Contact**: http://localhost:3000/contact
  - ✅ Contact info displays
  - ✅ Form is functional

- **Privacy**: http://localhost:3000/privacy
  - ✅ Policy content loads

- **Terms**: http://localhost:3000/terms
  - ✅ Terms content loads

### 3.3 Check Browser Console
- Open DevTools (F12)
- Go to Console tab
- Should have NO errors
- API calls should show in Network tab

## Managing Content

### Add/Edit Hero Sliders
```bash
# Via Laravel Tinker
php artisan tinker

# Create new hero slider
App\Models\HeroSlider::create([
    'title' => 'Your Title',
    'subtitle' => 'Your Subtitle',
    'description' => 'Your Description',
    'cta_text' => 'Button Text',
    'cta_link' => '/link',
    'image' => '🎯',
    'sort_order' => 1,
    'is_active' => true
]);

# Exit tinker
exit
```

### Add/Edit Page Content
```bash
php artisan tinker

# Create mission content
App\Models\PageContent::create([
    'page' => 'about',
    'section' => 'mission',
    'title' => 'Our Mission',
    'description' => 'Mission description',
    'sort_order' => 1,
    'is_active' => true
]);

exit
```

### Add/Edit Team Members
```bash
php artisan tinker

App\Models\TeamMember::create([
    'name' => 'John Doe',
    'position' => 'Lead Developer',
    'bio' => 'Expert in web development',
    'sort_order' => 1,
    'is_active' => true
]);

exit
```

### Add/Edit Contact Info
```bash
php artisan tinker

App\Models\ContactInfo::create([
    'type' => 'email',
    'label' => 'Email Us',
    'value' => 'contact@example.com',
    'description' => 'Response time',
    'sort_order' => 1,
    'is_active' => true
]);

exit
```

## Common Tasks

### View All Content
```bash
php artisan tinker

# View all active hero sliders
App\Models\HeroSlider::active()->get();

# View all page content
App\Models\PageContent::all();

# View specific page content
App\Models\PageContent::page('about')->get();

exit
```

### Disable/Enable Content
```bash
php artisan tinker

# Disable a hero slider
$slider = App\Models\HeroSlider::find(1);
$slider->update(['is_active' => false]);

# Enable a stat
$stat = App\Models\Stat::find(1);
$stat->update(['is_active' => true]);

exit
```

### Update Sort Order
```bash
php artisan tinker

# Update sort order
App\Models\HeroSlider::find(1)->update(['sort_order' => 2]);

exit
```

### Delete Content
```bash
php artisan tinker

App\Models\HeroSlider::find(1)->delete();

exit
```

## Troubleshooting

### Frontend not loading content?

1. **Check backend is running**
   ```bash
   curl http://localhost:8000/api/content/home
   ```
   Should return JSON data, not an error

2. **Check environment variables**
   ```bash
   # In frontend/.env.local
   cat .env.local
   ```
   Verify `NEXT_PUBLIC_API_URL=http://localhost:8000`

3. **Clear Next.js cache**
   ```bash
   rm -rf .next
   npm run dev
   ```

4. **Check browser console for errors**
   - Open DevTools (F12)
   - Check Console and Network tabs

### Database issues?

1. **Check database connection**
   ```bash
   php artisan tinker
   DB::connection()->getPdo();
   exit
   ```

2. **Check migrations ran**
   ```bash
   php artisan migrate:status
   ```

3. **Reset database**
   ```bash
   php artisan migrate:fresh --seed
   ```

### CORS errors?

1. **Verify CORS config**
   ```php
   // Check config/cors.php
   cat config/cors.php
   ```

2. **Enable CORS for development**
   ```php
   'allowed_origins' => ['http://localhost:3000'],
   ```

3. **Clear cache**
   ```bash
   php artisan config:cache
   ```

## Development Workflow

### Making Content Changes

1. Connect to backend database (via Tinker or admin panel)
2. Update content in database
3. Frontend automatically fetches updated content (no rebuild needed)
4. Changes appear instantly on the page

### Adding New Pages

1. Create new page component in `frontend/app/[page]/page.tsx`
2. Add API endpoint in backend
3. Fetch from API using utility functions
4. Add to navigation if needed

## API Documentation

### Home Content
```
GET /api/content/home
Returns: { hero_sliders, stats, features, categories, testimonials, faq }
```

### About Content
```
GET /api/content/about
Returns: { mission, vision, stats, team }
```

### Contact Content
```
GET /api/content/contact
Returns: { hero, contact_info, faq }
```

### Privacy Content
```
GET /api/content/privacy
Returns: { title, description, content }
```

### Terms Content
```
GET /api/content/terms
Returns: { title, description, content }
```

### Products
```
GET /api/products?page=1&per_page=12
Returns: { data, current_page, last_page, total, per_page }
```

## Environment Variables

### Frontend (`.env.local`)
```bash
# API connection
NEXT_PUBLIC_API_URL=http://localhost:8000
NEXT_PUBLIC_API_BASE_PATH=/api
```

### Backend (`.env`)
```bash
# Database
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=nextdigihome
DB_USERNAME=root
DB_PASSWORD=

# CORS
CORS_PATHS=api/*,sanctum/csrf-cookie
CORS_ALLOWED_ORIGINS=http://localhost:3000
```

## Next Steps

1. ✅ Get backend and frontend running
2. ✅ Verify content loads on pages
3. ✅ Try adding/editing content via Tinker
4. ✅ Build admin panel for easier content management
5. ✅ Deploy to production

## Support

For detailed information, see:
- [Frontend Implementation Guide](DYNAMIC_CONTENT_GUIDE.md) - Frontend setup details
- [Backend Setup Guide](../backend/BACKEND_SETUP_GUIDE.md) - Database and API setup

## Key Files

### Frontend
- `.env.local` - Environment configuration
- `app/utils/api.ts` - API utility functions
- `app/page.tsx` - Home page
- `app/about/page.tsx` - About page
- `app/contact/page.tsx` - Contact page
- `app/privacy/page.tsx` - Privacy page
- `app/terms/page.tsx` - Terms page

### Backend
- `app/Models/` - Data models
- `routes/api.php` - API routes
- `app/Http/Controllers/Api/` - API controllers
- `database/migrations/` - Database structure
- `database/seeders/` - Data seeders

---

**Happy coding! 🎉**

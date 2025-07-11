# Neeraj Gupta – Portfolio

Hello everyone! 👋

I’m **Neeraj Gupta**. I’d like to share my personal portfolio website project that I’ve developed.

## 🚀 Live Demo

**Website Link:** [https://neeraj-portfolio.com](https://neeraj-portfolio.com)

*(Replace the domain above with your final deployed URL once it’s live on Vercel.)*

## 🛠️ Tech Stack

This project is built using modern web technologies:

- **ReactJS** – Frontend framework
- **Tailwind CSS** – Utility-first CSS framework
- **Supabase** – Backend for portfolio data, certificates, and comment system
- **AOS** – Animate On Scroll library
- **Framer Motion** – Animation library
- **Lucide** – Icon library
- **Material UI** – React component library
- **SweetAlert2** – Beautiful alert dialogs

## 📋 Prerequisites

Before running this project, ensure you have the following installed:

- **Node.js** (version 14.x or higher)
- **npm** or **yarn** package manager

## 🏃‍♂️ Getting Started

Follow these steps to run the project locally:

### 1. Clone the Repository

```bash
git clone https://github.com/neerajgupta/Portfolio_V5.git
cd Portfolio_V5
```

### 2. Install Dependencies
```bash
npm install
```

If you encounter peer dependency issues, use:

```bash
npm install --legacy-peer-deps
```

### 3. Run the Development Server
```bash
npm run dev
```

### 4. Open in Browser
Access the application through the link displayed in your terminal (usually http://localhost:5173).

## 🏗️ Building for Production

To create a production-ready build:

Run the build command:

```bash
npm run build
```

The build files will be saved in the dist folder. Upload this folder to your hosting server (e.g. Vercel).

## ⚙️ Configuration (Supabase)
All backend data for this project (portfolio items, certificates, and comments) is managed by Supabase.

### 1. Create a Supabase Project
Go to Supabase and create a new project.

Keep your Project URL and anon public key handy. You can find them under Settings > API.

### 2. Setup Database Tables & Policies
Run the following all-in-one SQL script in your Supabase SQL Editor. This will create all necessary tables, policies, storage, and insert example data.

```sql
-- ---- TABLE CREATION ----

CREATE TABLE public.projects (
  id bigint GENERATED BY DEFAULT AS IDENTITY PRIMARY KEY,
  created_at timestamp with time zone DEFAULT now() NOT NULL,
  "Title" text,
  "Description" text,
  "Img" text,
  "Link" text,
  "Github" text,
  "Features" jsonb,
  "TechStack" jsonb
);

CREATE TABLE public.certificates (
  id bigint GENERATED BY DEFAULT AS IDENTITY PRIMARY KEY,
  created_at timestamp with time zone DEFAULT now() NOT NULL,
  "Img" text
);

CREATE TABLE public.portfolio_comments (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  content TEXT NOT NULL,
  user_name VARCHAR(255) NOT NULL,
  profile_image TEXT,
  is_pinned BOOLEAN DEFAULT FALSE,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- ---- ROW LEVEL SECURITY (RLS) ----

ALTER TABLE public.projects ENABLE ROW LEVEL SECURITY;
ALTER TABLE public.certificates ENABLE ROW LEVEL SECURITY;
ALTER TABLE public.portfolio_comments ENABLE ROW LEVEL SECURITY;

-- ---- POLICIES ----

CREATE POLICY "Public Read Access for Projects"
ON public.projects FOR SELECT TO public USING (true);

CREATE POLICY "Public Read Access for Certificates"
ON public.certificates FOR SELECT TO public USING (true);

CREATE POLICY "Allow public read on portfolio_comments"
ON public.portfolio_comments FOR SELECT TO public USING (true);

CREATE POLICY "Allow public insert on portfolio_comments"
ON public.portfolio_comments FOR INSERT TO public WITH CHECK (is_pinned = false);

-- ---- STORAGE ----

INSERT INTO storage.buckets (id, name, public)
VALUES ('profile-images', 'profile-images', true)
ON CONFLICT (id) DO NOTHING;

CREATE POLICY "Allow public to upload profile images"
ON storage.objects FOR INSERT TO public WITH CHECK (bucket_id = 'profile-images');

CREATE POLICY "Allow public to read profile images"
ON storage.objects FOR SELECT TO public USING (bucket_id = 'profile-images');

-- ---- EXAMPLE DATA ----

INSERT INTO public.projects ("Title", "Description", "Img", "Link", "Github", "Features", "TechStack")
VALUES (
    'Example Project Title',
    'A simple description for this example project, explaining its main purpose and goals.',
    'REPLACE_WITH_YOUR_PROJECT_IMAGE_URL.png',
    'REPLACE_WITH_YOUR_LIVE_DEMO_URL.com',
    'REPLACE_WITH_YOUR_GITHUB_REPO_URL.com',
    '["Main Feature A", "Core Function B", "Key Ability C"]',
    '["React", "Supabase", "Tailwind CSS"]'
);

INSERT INTO public.certificates ("Img")
VALUES ('REPLACE_WITH_YOUR_CERTIFICATE_IMAGE_URL.png');

INSERT INTO public.portfolio_comments (content, user_name)
VALUES ('Created by Neeraj Gupta', 'Neeraj Gupta');
```

### 3. Enable Realtime for Comments
Go to Database > Replication.

Click the “0 tables” text.

Enable replication for the portfolio_comments table. This allows your comment section to update in real-time.

### 🔧 Environment Variables
Create a file named .env in your project root and add your Supabase credentials:

```env
# Supabase Configuration
VITE_SUPABASE_URL=your-supabase-url
VITE_SUPABASE_ANON_KEY=your-supabase-anon-key
```

**Important notes:**

- All variables must be prefixed with `VITE_` for Vite to access them.
- Restart the dev server after changes.
- Never commit your `.env` file. Add it to `.gitignore`.


## ⚙️ Supabase Config File (supabase.js)

Ensure your Supabase client uses these variables:

```js
import { createClient } from "@supabase/supabase-js";

const supabaseUrl = import.meta.env.VITE_SUPABASE_URL;
const supabaseKey = import.meta.env.VITE_SUPABASE_ANON_KEY;

if (!supabaseUrl || !supabaseKey) {
  throw new Error("Supabase URL and Anon Key are required. Check your .env file.");
}

export const supabase = createClient(supabaseUrl, supabaseKey);
```
## 🚨 Troubleshooting
If you run into problems:

- Confirm Node.js is installed correctly.
- Verify you’re in the right folder.
- Check that all dependencies install without errors.
- Double-check your Supabase `.env` variables.
- Clear your browser cache if issues persist.


## 📝 Usage & Credits
Feel free to use this project for learning or inspiration. Please credit me if you decide to fork or deploy it publicly.

Thank you! 🙏


## 📞 Contact
If you have questions or need help setting up the project, feel free to reach out!

Neeraj Gupta

- Website: [https://neeraj-portfolio.com](https://neeraj-portfolio.com)

- GitHub: [neerajgupta](https://github.com/NeerajGupta-dev/)

⭐ If you found this project helpful, consider giving it a star on GitHub!

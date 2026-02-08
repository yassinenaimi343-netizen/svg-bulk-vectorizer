# Railway Deployment - Complete Step-by-Step Guide

This guide shows you exactly where to click and what to do to deploy your SVG Bulk Vectorizer on Railway.

---

## STEP 1: Create Railway Account

1. Go to https://railway.app
2. Click **"Start Free"** button (top right)
3. Click **"Sign up with GitHub"**
4. Authorize Railway to access your GitHub account
5. You'll be logged in and see the Railway dashboard

---

## STEP 2: Create New Project

1. On Railway dashboard, click **"New Project"** button (top right)
2. A dropdown menu appears - click **"Deploy from GitHub"**
3. You'll see a list of your GitHub repositories
4. Find **"svg-bulk-vectorizer"** in the list
5. Click on it to select it
6. Click **"Deploy Now"** button

Railway will start building your project. This takes 2-5 minutes. You'll see a progress bar.

---

## STEP 3: Add MySQL Database

While the build is happening, let's add the database:

1. In the Railway project page, look for **"Add Service"** button (usually at bottom left)
2. Click **"Add Service"**
3. Click **"Add from Marketplace"**
4. Search for **"MySQL"** in the search box
5. Click on **"MySQL"** result
6. Click **"Create"** or **"Add"**

Railway will provision a MySQL database. Wait for it to finish (usually 1-2 minutes).

---

## STEP 4: Get Database Connection String

1. In your Railway project, you should now see two services:
   - Your app (svg-bulk-vectorizer)
   - MySQL database

2. Click on the **"MySQL"** service box
3. On the right side, you'll see a panel with details
4. Look for **"DATABASE_URL"** variable
5. Click the **copy icon** next to it to copy the full connection string
6. **Save this somewhere safe** - you'll need it in the next step

---

## STEP 5: Configure Environment Variables

1. Click on your **app service** (the svg-bulk-vectorizer box, not MySQL)
2. On the right panel, click the **"Variables"** tab
3. You'll see a list of variables. Click **"Add Variable"** or the **"+"** button

Now add these variables one by one:

### Variable 1: DATABASE_URL
- **Name**: `DATABASE_URL`
- **Value**: Paste the MySQL connection string you copied in Step 4
- Click **"Add"** or **"Save"**

### Variable 2: NODE_ENV
- **Name**: `NODE_ENV`
- **Value**: `production`
- Click **"Add"**

### Variable 3: JWT_SECRET
- **Name**: `JWT_SECRET`
- **Value**: `your-secret-key-12345-change-this` (can be any random string)
- Click **"Add"**

### Variable 4: STRIPE_SECRET_KEY
- **Name**: `STRIPE_SECRET_KEY`
- **Value**: Get from https://dashboard.stripe.com/apikeys (copy "Secret key")
- Click **"Add"**

### Variable 5: STRIPE_PUBLISHABLE_KEY
- **Name**: `STRIPE_PUBLISHABLE_KEY`
- **Value**: Get from https://dashboard.stripe.com/apikeys (copy "Publishable key")
- Click **"Add"**

### Variable 6: VITE_APP_ID
- **Name**: `VITE_APP_ID`
- **Value**: `svg-bulk-vectorizer` (or your app ID)
- Click **"Add"**

### Variable 7: VITE_APP_TITLE
- **Name**: `VITE_APP_TITLE`
- **Value**: `SVG Bulk Vectorizer`
- Click **"Add"**

### Variable 8: OWNER_NAME
- **Name**: `OWNER_NAME`
- **Value**: Your name
- Click **"Add"**

**Note**: For OAuth variables (OAUTH_SERVER_URL, VITE_OAUTH_PORTAL_URL, etc.), you can leave them as defaults or configure your own OAuth provider later.

---

## STEP 6: Wait for Deployment

1. Go back to your project page
2. Look at the **app service** box
3. You should see a **green checkmark** or **"Deployed"** status
4. If you see **"Building"** or **"Deploying"**, wait for it to finish

This usually takes 3-5 minutes.

---

## STEP 7: Get Your Railway URL

1. Click on your **app service** box
2. On the right panel, look for **"Deployments"** tab
3. Click on the latest deployment (top one)
4. Look for a section that shows your **public URL**
5. It will look like: `https://svg-bulk-vectorizer-production-xxxx.railway.app`
6. Click the **copy icon** to copy it
7. **This is your temporary URL** - you can access your app here!

---

## STEP 8: Test Your App

1. Open the URL you copied in a new browser tab
2. You should see the SVG Bulk Vectorizer homepage
3. Test these features:
   - Upload a test image
   - Convert it to SVG
   - Download the SVG
   - Try changing colors
   - Try exporting as PDF

If everything works, your deployment is successful!

---

## STEP 9: Add Custom Domain (Optional but Recommended)

If you have a custom domain (like vectorizer.yourdomain.com):

1. In Railway project, click your **app service**
2. On the right panel, look for **"Settings"** tab
3. Scroll down to find **"Domains"** section
4. Click **"Add Domain"**
5. Enter your domain name (e.g., `vectorizer.yourdomain.com`)
6. Railway will show you **DNS records** to add
7. Go to your domain registrar (GoDaddy, Namecheap, etc.)
8. Add the DNS records Railway provided
9. Wait 5-30 minutes for DNS to propagate
10. Your app will be accessible at your custom domain!

---

## STEP 10: Configure Stripe Webhooks

If you want payment processing to work:

1. Go to https://dashboard.stripe.com/webhooks
2. Click **"Add endpoint"**
3. In the URL field, enter: `https://your-railway-url.railway.app/api/webhooks/stripe`
   - Replace `your-railway-url` with your actual Railway URL
4. Select these events:
   - `payment_intent.succeeded`
   - `customer.subscription.updated`
   - `customer.subscription.deleted`
5. Click **"Add endpoint"**
6. Stripe will show you a **"Signing secret"**
7. Copy it and add to Railway variables:
   - **Name**: `STRIPE_WEBHOOK_SECRET`
   - **Value**: Paste the signing secret
   - Click **"Add"**

---

## FINAL RESULT

Your SVG Bulk Vectorizer is now live! 

**Your URL**: `https://your-railway-url.railway.app` (or your custom domain)

You can now:
- Share this URL with others
- Upload and convert images
- Download in multiple formats
- Use all features (color editing, bulk operations, etc.)

---

## Troubleshooting

### App shows error or won't load
- Check Railway dashboard for build errors (red X)
- Look at deployment logs (click deployment → Logs tab)
- Verify all environment variables are set correctly

### Database connection error
- Verify DATABASE_URL is correct
- Check MySQL service is running (green checkmark)
- Ensure MySQL credentials match

### Images won't convert
- Check browser console (F12) for errors
- Verify Potrace WASM is loading
- Check Railway logs for backend errors

### Stripe not working
- Verify STRIPE_SECRET_KEY and STRIPE_PUBLISHABLE_KEY are correct
- Check webhook endpoint is correct in Stripe dashboard
- Look at Railway logs for Stripe errors

---

## Next Steps

1. **Monitor your app**: Check Railway dashboard regularly for errors
2. **Set up monitoring**: Enable Railway's monitoring features
3. **Plan for scaling**: If you get many users, upgrade Railway plan
4. **Backup database**: Set up automated MySQL backups
5. **Add more features**: Consider adding analytics, user accounts, etc.

---

## Important Links

- Railway Dashboard: https://railway.app/dashboard
- Stripe Dashboard: https://dashboard.stripe.com
- Your GitHub Repo: Check your GitHub for deployment logs
- Railway Docs: https://docs.railway.app

---

## Support

If you get stuck:
1. Check the troubleshooting section above
2. Look at Railway deployment logs
3. Check your browser console (F12) for errors
4. Visit Railway docs: https://docs.railway.app/help

Good luck! 🚀

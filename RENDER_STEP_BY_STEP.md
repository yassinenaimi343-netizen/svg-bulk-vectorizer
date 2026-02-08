# Render Deployment - Complete Step-by-Step Guide

This guide shows you exactly where to click and what to do to deploy your SVG Bulk Vectorizer on Render.

---

## STEP 1: Create Render Account

1. Go to https://render.com
2. Click **"Get Started"** button (top right)
3. Click **"Sign up with GitHub"**
4. Authorize Render to access your GitHub account
5. You'll be logged in and see the Render dashboard

---

## STEP 2: Create MySQL Database

First, let's set up the database:

1. On Render dashboard, click **"New +"** button (top right)
2. Click **"MySQL"** from the dropdown menu
3. Fill in the form:
   - **Name**: `svg-bulk-vectorizer-db`
   - **Database**: `svg_bulk_vectorizer`
   - **Username**: `admin` (or your choice)
   - **Password**: Generate a strong password (copy it somewhere safe!)
   - **Region**: Choose closest to you
   - **Plan**: Free tier is fine for testing

4. Click **"Create Database"**

Wait 2-3 minutes for the database to be created. You'll see a green checkmark when ready.

---

## STEP 3: Get Database Connection Details

1. Click on your newly created MySQL database
2. On the right side, you'll see connection details:
   - **Hostname**
   - **Port** (usually 3306)
   - **Database name**
   - **Username**
   - **Password**

3. Construct your DATABASE_URL in this format:
   ```
   mysql://username:password@hostname:port/database_name
   ```
   
   Example:
   ```
   mysql://admin:mypassword123@dpg-xxx.render.ondigitalocean.com:3306/svg_bulk_vectorizer
   ```

4. **Save this DATABASE_URL** - you'll need it in the next step

---

## STEP 4: Create Web Service

Now let's deploy your app:

1. On Render dashboard, click **"New +"** button (top right)
2. Click **"Web Service"** from the dropdown
3. You'll see a list of your GitHub repositories
4. Find **"svg-bulk-vectorizer"** and click **"Connect"**
5. Fill in the form:
   - **Name**: `svg-bulk-vectorizer`
   - **Environment**: `Node`
   - **Build Command**: `npm install && npm run build`
   - **Start Command**: `npm start`
   - **Plan**: Free tier (or upgrade if needed)
   - **Region**: Same as your database

6. Click **"Create Web Service"**

Render will start building your app. This takes 5-10 minutes. You'll see build logs in real-time.

---

## STEP 5: Add Environment Variables

While the build is happening, let's add environment variables:

1. In your Web Service page, scroll down to **"Environment"** section
2. Click **"Add Environment Variable"** button

Add these variables one by one:

### Variable 1: DATABASE_URL
- **Key**: `DATABASE_URL`
- **Value**: Paste the DATABASE_URL you created in Step 3
- Click **"Save"**

### Variable 2: NODE_ENV
- **Key**: `NODE_ENV`
- **Value**: `production`
- Click **"Save"**

### Variable 3: JWT_SECRET
- **Key**: `JWT_SECRET`
- **Value**: `your-secret-key-12345-change-this` (any random string)
- Click **"Save"**

### Variable 4: STRIPE_SECRET_KEY
- **Key**: `STRIPE_SECRET_KEY`
- **Value**: Get from https://dashboard.stripe.com/apikeys (copy "Secret key")
- Click **"Save"**

### Variable 5: STRIPE_PUBLISHABLE_KEY
- **Key**: `STRIPE_PUBLISHABLE_KEY`
- **Value**: Get from https://dashboard.stripe.com/apikeys (copy "Publishable key")
- Click **"Save"**

### Variable 6: VITE_APP_ID
- **Key**: `VITE_APP_ID`
- **Value**: `svg-bulk-vectorizer`
- Click **"Save"**

### Variable 7: VITE_APP_TITLE
- **Key**: `VITE_APP_TITLE`
- **Value**: `SVG Bulk Vectorizer`
- Click **"Save"**

### Variable 8: OWNER_NAME
- **Key**: `OWNER_NAME`
- **Value**: Your name
- Click **"Save"**

### Variable 9: PORT
- **Key**: `PORT`
- **Value**: `3000`
- Click **"Save"**

**Note**: After adding each variable, Render will automatically redeploy your app.

---

## STEP 6: Wait for Deployment

1. Go back to your Web Service page
2. Look at the **"Build & Deployment"** section
3. You should see **"Build in progress"** or **"Deploying"**
4. Wait for it to complete (you'll see a green checkmark and "Live" status)

This usually takes 5-10 minutes total.

---

## STEP 7: Get Your Render URL

1. On your Web Service page, look at the top
2. You'll see a URL like: `https://svg-bulk-vectorizer.onrender.com`
3. Click the **copy icon** to copy it
4. **This is your live URL!**

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

## STEP 9: Add Custom Domain (Optional)

If you have a custom domain (like vectorizer.yourdomain.com):

1. On your Web Service page, scroll to **"Settings"** section
2. Look for **"Custom Domain"** option
3. Click **"Add Custom Domain"**
4. Enter your domain name (e.g., `vectorizer.yourdomain.com`)
5. Render will show you **CNAME record** to add
6. Go to your domain registrar (GoDaddy, Namecheap, etc.)
7. Add the CNAME record Render provided
8. Wait 5-30 minutes for DNS to propagate
9. Your app will be accessible at your custom domain!

---

## STEP 10: Configure Stripe Webhooks

If you want payment processing to work:

1. Go to https://dashboard.stripe.com/webhooks
2. Click **"Add endpoint"**
3. In the URL field, enter: `https://your-render-url.onrender.com/api/webhooks/stripe`
   - Replace `your-render-url` with your actual Render URL
4. Select these events:
   - `payment_intent.succeeded`
   - `customer.subscription.updated`
   - `customer.subscription.deleted`
5. Click **"Add endpoint"**
6. Stripe will show you a **"Signing secret"**
7. Copy it and add to Render environment variables:
   - **Key**: `STRIPE_WEBHOOK_SECRET`
   - **Value**: Paste the signing secret
   - Click **"Save"**

---

## STEP 11: Set Up Auto-Deploy from GitHub

Your app will automatically redeploy when you push to GitHub:

1. On your Web Service page, look for **"Auto-Deploy"** section
2. Make sure it's set to **"Yes"** (should be by default)
3. Select the branch: **"main"**
4. Now every time you push to GitHub, Render will automatically rebuild and deploy!

---

## FINAL RESULT

Your SVG Bulk Vectorizer is now live on Render!

**Your URL**: `https://svg-bulk-vectorizer.onrender.com` (or your custom domain)

You can now:
- Share this URL with others
- Upload and convert images
- Download in multiple formats
- Use all features (color editing, bulk operations, etc.)

---

## Troubleshooting

### App shows error or won't load
- Check Render dashboard for build errors (red X)
- Look at deployment logs (click "Logs" tab)
- Verify all environment variables are set correctly

### Database connection error
- Verify DATABASE_URL is correct
- Check MySQL database is running (green checkmark)
- Ensure MySQL credentials match

### Images won't convert
- Check browser console (F12) for errors
- Verify Potrace WASM is loading
- Check Render logs for backend errors

### Stripe not working
- Verify STRIPE_SECRET_KEY and STRIPE_PUBLISHABLE_KEY are correct
- Check webhook endpoint is correct in Stripe dashboard
- Look at Render logs for Stripe errors

### App keeps redeploying
- Check if you're pushing to GitHub frequently
- Disable auto-deploy if you want to test locally first
- Look at deployment logs to see what triggered the rebuild

---

## Render vs Railway Comparison

| Feature | Render | Railway |
|---------|--------|---------|
| Free Tier | Yes (limited) | Yes (limited) |
| MySQL Database | Yes | Yes |
| Auto-Deploy | Yes | Yes |
| Custom Domain | Yes | Yes |
| Pricing | Pay-as-you-go | Pay-as-you-go |
| Uptime | 99.9% | 99.9% |
| Support | Good | Good |

Both are excellent choices. Render is slightly easier for beginners.

---

## Next Steps

1. **Monitor your app**: Check Render dashboard regularly for errors
2. **Set up monitoring**: Enable Render's monitoring features
3. **Plan for scaling**: If you get many users, upgrade Render plan
4. **Backup database**: Set up automated MySQL backups
5. **Add more features**: Consider adding analytics, user accounts, etc.

---

## Important Links

- Render Dashboard: https://dashboard.render.com
- Stripe Dashboard: https://dashboard.stripe.com
- Your GitHub Repo: Check your GitHub for deployment logs
- Render Docs: https://render.com/docs

---

## Support

If you get stuck:
1. Check the troubleshooting section above
2. Look at Render deployment logs
3. Check your browser console (F12) for errors
4. Visit Render docs: https://render.com/docs

Good luck! 🚀

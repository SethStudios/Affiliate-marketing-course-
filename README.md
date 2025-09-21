# Affiliate Marketing Course Website Setup

# Affiliate Marketing Course Website

## New Payment-First Flow 🔄

### User Journey:
1. **Homepage** → User clicks "Enroll Now for ₦3,500"
2. **Payment Page** → User enters details and pays via Paystack
3. **Signup Page** → User creates password (email pre-filled from payment)
4. **Email Verification** → User verifies email
5. **Login & Course Access** → User can access course content

### Benefits:
- ✅ No duplicate account errors
- ✅ Payment verified before account creation
- ✅ Cleaner user experience
- ✅ Reduced email verification conflicts

## Important: Paystack Configuration Required

Before the payment functionality works, you need to:

1. **Get Your Paystack Keys:**
   - Sign up at https://paystack.com if you haven't already
   - Go to Dashboard > Settings > API Keys & Webhooks
   - Copy your **Public Key** (starts with `pk_test_` for test mode)

2. **Update the JavaScript File:**
   - Open `script.js`
   - Find line 2: `const PAYSTACK_PUBLIC_KEY = 'pk_test_your_public_key_here';`
   - Replace `'pk_test_your_public_key_here'` with your actual Paystack public key

## To Test the Website:

### Option 1: Simple File Opening
- Double-click `index.html` to open in your browser
- Note: Payment won't work with file:// protocol, but you can see the design

### Option 2: Using a Local Server (Recommended)
If you have Node.js installed:
```bash
npx http-server . -p 8000 -c-1
```

If you have Python installed:
```bash
python -m http.server 8000
```

Then visit: http://localhost:8000

## Features Included:

✅ **Dark Theme Design** - Modern, professional look
✅ **Fully Responsive** - Works on desktop, tablet, and mobile
✅ **Paystack Integration** - Secure payment processing for ₦3,500
✅ **User Experience** - Smooth animations and interactive elements
✅ **Course Preview** - Shows the 5 main modules
✅ **Loading States** - Professional payment flow
✅ **Email Capture** - Prompts for user email before payment
✅ **Success Handling** - Shows confirmation after payment
✅ **Error Handling** - Graceful handling if payment is cancelled

## Next Steps:

1. **Replace Paystack Key** (as mentioned above)
2. **Customize Content** - Update course modules, pricing, or description
3. **Add Backend** (optional) - For payment verification and course delivery
4. **Deploy** - Upload to a web hosting service

## File Structure:
```
├── index.html      # Main homepage
├── styles.css      # All styling and responsive design  
├── script.js       # Paystack integration and interactions
└── README.md       # This setup guide
```

The website is ready to use! Just update your Paystack key and you're good to go. 🚀
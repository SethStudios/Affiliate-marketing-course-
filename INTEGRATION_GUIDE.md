# Complete Paystack + Supabase Integration Setup Guide

## 🚀 Overview

This guide will help you set up a complete affiliate marketing course website with:
- **Paystack payment processing** for ₦3,500 course enrollment
- **Supabase authentication** for user account creation
- **Database integration** for student management
- **Payment verification** and error handling

## 📋 Prerequisites

1. **Paystack Account**: Sign up at [paystack.com](https://paystack.com)
2. **Supabase Account**: Sign up at [supabase.com](https://supabase.com)
3. **Web Server**: For testing (Node.js, Python, or any local server)

## ⚙️ Step 1: Paystack Configuration

### 1.1 Get Your Paystack Keys
1. Log into your Paystack dashboard
2. Go to **Settings > API Keys & Webhooks**
3. Copy your:
   - **Public Key** (starts with `pk_test_` for test mode)
   - **Secret Key** (starts with `sk_test_` for test mode)

### 1.2 Update Configuration
Open `config.js` and update:
```javascript
PAYSTACK: {
    PUBLIC_KEY: 'pk_test_your_actual_public_key_here',
    SECRET_KEY: 'sk_test_your_actual_secret_key_here', // Keep this secure!
    VERIFY_URL: 'https://api.paystack.co/transaction/verify/'
}
```

## 🗄️ Step 2: Supabase Setup

### 2.1 Create New Project
1. Log into Supabase dashboard
2. Click "New Project"
3. Choose organization and enter project details
4. Wait for setup to complete (2-3 minutes)

### 2.2 Get Supabase Credentials
1. Go to **Settings > API**
2. Copy your:
   - **Project URL**
   - **Anon/Public Key**

### 2.3 Update Configuration
Open `config.js` and update:
```javascript
SUPABASE: {
    URL: 'https://your-project-id.supabase.co',
    ANON_KEY: 'your_supabase_anon_key_here'
}
```

### 2.4 Create Students Table
Run this SQL in Supabase SQL Editor:

```sql
-- Create students table
CREATE TABLE students (
    id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
    user_id UUID REFERENCES auth.users(id),
    email VARCHAR(255) NOT NULL,
    full_name VARCHAR(255) NOT NULL,
    has_paid BOOLEAN DEFAULT false,
    payment_reference VARCHAR(255) UNIQUE,
    payment_amount DECIMAL(10,2),
    course_name VARCHAR(255) DEFAULT 'Affiliate Marketing Mastery',
    enrolled_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Enable Row Level Security
ALTER TABLE students ENABLE ROW LEVEL SECURITY;

-- Create policy for authenticated users
CREATE POLICY "Users can view their own records" ON students
    FOR SELECT USING (auth.uid() = user_id);

-- Create policy for inserting records
CREATE POLICY "Users can insert their own records" ON students
    FOR INSERT WITH CHECK (auth.uid() = user_id);

-- Add index for better performance
CREATE INDEX idx_students_email ON students(email);
CREATE INDEX idx_students_payment_ref ON students(payment_reference);
```

## 🔧 Step 3: Testing the Integration

### 3.1 Test Payment Flow
1. Open the website in your browser
2. Click "Enroll Now for ₦3,500"
3. Enter email, password, and full name when prompted
4. Complete test payment using Paystack test cards:
   - **Success**: `4084084084084081`
   - **Decline**: `4000000000000002`

### 3.2 Verify Database Records
After successful payment:
1. Check Supabase **Authentication > Users** for new user account
2. Check **Table Editor > students** for enrollment record
3. Verify `has_paid` is set to `true`

## 🚦 Step 4: Production Deployment

### 4.1 Switch to Live Keys
1. Replace test keys with live Paystack keys (starting with `pk_live_` and `sk_live_`)
2. Test thoroughly in staging environment
3. Update webhook URLs if using backend verification

### 4.2 Security Considerations
- **Never expose secret keys** in frontend code
- Use **environment variables** for sensitive data
- Implement **backend payment verification** for production
- Enable **Supabase RLS policies** for data security

## 📊 Step 5: Analytics & Monitoring

### 5.1 Payment Tracking
The system includes Google Analytics integration:
```javascript
// Automatic purchase event tracking
gtag('event', 'purchase', {
    transaction_id: paymentResponse.reference,
    value: 3500, // Course price in naira
    currency: 'NGN'
});
```

### 5.2 Error Monitoring
Monitor these key metrics:
- Payment success/failure rates
- User account creation success
- Database insertion errors
- API response times

## 🔍 Troubleshooting

### Common Issues:

1. **"Paystack configuration incomplete"**
   - Verify public key starts with `pk_test_` or `pk_live_`
   - Check for typos in config.js

2. **"Supabase not initialized"**
   - Verify Supabase URL and anon key
   - Check browser console for CORS errors

3. **"User creation failed"**
   - Check email format validation
   - Verify password meets requirements (8+ characters)
   - Check Supabase auth settings

4. **"Student record creation failed"**
   - Verify students table exists
   - Check RLS policies
   - Ensure user_id is properly set

## 📁 File Structure

```
Affiliate Marketing Course/
├── index.html              # Main homepage
├── styles.css              # Styling and animations
├── script.js               # Enhanced Paystack + Supabase integration
├── config.js               # Configuration file (update with your keys)
├── README.md               # Original setup guide
└── INTEGRATION_GUIDE.md    # This comprehensive guide
```

## 🎯 Next Steps

1. **Customize Course Content**: Update modules, pricing, descriptions
2. **Add Course Dashboard**: Create protected area for enrolled students
3. **Email Integration**: Set up welcome emails and course materials
4. **Advanced Features**: Add course progress tracking, certificates, etc.

## 🆘 Support

If you encounter issues:
1. Check browser console for JavaScript errors
2. Verify all configuration keys are correct
3. Test with Paystack test cards first
4. Review Supabase logs for database errors

Your affiliate marketing course website is now ready for production with full payment processing and user management! 🎉
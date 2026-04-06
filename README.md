# Winelands Periodontics - Patient Onboarding Dashboard

🦷 Smart, branded patient intake system with ElevenLabs voice agent integration, Typeform questionnaires, and Heidi Health scribe support.

## Features

✅ **5-Step Intelligent Onboarding Flow**
- Practitioner selection (Dr Inu vs Caitlin)
- Patient information collection
- Dynamic medical history forms
- Optional AI voice consultation
- Final review & consent

✅ **Branded Design**
- Gold/cream color scheme matching Winelands Periodontics brand
- Fully responsive (mobile, tablet, desktop)
- Smooth animations and progress tracking
- Accessibility compliant

✅ **ElevenLabs Integration**
- AI voice agent for patient pre-screening
- Natural conversation flow
- Optional participation (can skip)
- Integrated directly in onboarding

✅ **Dynamic Forms**
- Different forms for Dr Inus Snyman (specialist)
- Different forms for Caitlin Taute (oral hygiene)
- Conditional logic and field validation
- Medical history per practitioner

✅ **POPIA Compliant**
- POPIA 2013 acknowledgment
- Radiograph consent
- Photography consent
- Treatment consent
- Data privacy notices

## Quick Start

### Deploy to Your Website

1. **Copy the `index.html` file** to your website root directory
2. **Update configuration** (optional):
   - Modify the ElevenLabs agent ID if different
   - Customize colors in the `:root` CSS variables
3. **Access the dashboard** at: `https://winelandsperio.co.za` or your domain

### Local Testing

```bash
# Clone the repository
git clone https://github.com/XEQTAI/winelands-periodontics-dashboard.git
cd winelands-periodontics-dashboard

# Open in browser
open index.html
# or
python -m http.server 8000
# Then visit http://localhost:8000
```

## Integration Guide

### 1. ElevenLabs Voice Agent

**Current Setup:**
- Agent ID: `agent_6801kn6sbgdqekn9k3f2znesf7kq`
- Opens in new window when user clicks "Start Voice Call"
- Optional step (users can skip)

**To customize:**
```javascript
// In startVoiceCall() function, change the agent ID:
const elevenLabsAgentId = 'YOUR_AGENT_ID';
```

### 2. Form Data Handling

**Current Behavior:**
- Form data stored in browser localStorage
- Available in console: `localStorage.getItem('winelandsPatientData')`

**To integrate with backend:**

```javascript
// Replace the submitForm() function:
function submitForm() {
    // Send to your backend
    fetch('/api/patient-onboarding', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(formData)
    })
    .then(res => res.json())
    .then(data => {
        // Show success modal
        document.getElementById('successModal').classList.add('active');
    })
    .catch(err => console.error('Error:', err));
}
```

### 3. Typeform Integration

**Option A: Embed in Dashboard** (Recommended)
```html
<!-- Add after step 3 -->
<div class="form-group">
    <div data-tf-live="01HQXXX..."></div>
    <script src="//embed.typeform.com/next/embed.js"></script>
</div>
```

**Option B: Redirect to Typeform**
```javascript
// In step 3, redirect to Typeform:
window.location.href = 'https://yourcompany.typeform.com/to/...';
```

### 4. Heidi Health Scribe Integration

**Webhook Setup:**
```javascript
// When form is submitted, send to Heidi Health:
async function submitToHeidhHealth(patientData) {
    const response = await fetch('https://api.heidhealth.com/v1/encounters', {
        method: 'POST',
        headers: {
            'Authorization': 'Bearer YOUR_API_KEY',
            'Content-Type': 'application/json'
        },
        body: JSON.stringify({
            patient_name: patientData.firstName + ' ' + patientData.lastName,
            patient_email: patientData.email,
            patient_phone: patientData.phone,
            appointment_date: patientData.appointmentDate,
            medical_history: patientData.medicalHistory,
            practitioner: patientData.practitioner === 'dr_inu' ? 'Dr Inus Snyman' : 'Caitlin Taute'
        })
    });
    return response.json();
}
```

## Customization

### Colors

Edit the CSS `:root` variables in `index.html`:

```css
:root {
    --primary-gold: #B8956A;      /* Main brand color */
    --dark-gold: #8B7355;         /* Darker accent */
    --light-cream: #F5F1E8;       /* Background */
    --white: #FFFFFF;             /* Cards */
    --text-dark: #2C2C2C;         /* Text */
    --success: #4CAF50;           /* Success state */
}
```

### Forms

Edit the `practitionerConfigs` object in `index.html` to customize questions:

```javascript
const practitionerConfigs = {
    dr_inu: {
        name: 'Dr Inus Snyman',
        fields: [
            {
                name: 'mainComplaint',
                label: 'Main Complaint',
                type: 'textarea',
                required: true
            },
            // Add more fields...
        ]
    }
}
```

### Contact Information

Update practice details:
- **Address:** 27 Saffraan Ave, Stellenbosch
- **Phone:** 021 883 3412
- **Email:** info@drisnyman.co.za
- **Website:** www.winelandsperio.co.za

## Browser Support

✅ Chrome/Edge 90+
✅ Firefox 88+
✅ Safari 14+
✅ Mobile browsers (iOS Safari, Chrome Mobile)

## Mobile Experience

- Fully responsive design
- Touch-friendly buttons and inputs
- Mobile optimized progress indicators
- Optimized for tablet use in waiting room

## Data Privacy

✅ POPIA 2013 Compliant
✅ No external data transmission (unless configured)
✅ Client-side form validation
✅ Encrypted storage options available

## Deployment

### GitHub Pages (Free)

```bash
git push origin main
```

Then enable GitHub Pages in repository settings.

Your site will be available at: `https://XEQTAI.github.io/winelands-periodontics-dashboard/`

### Custom Domain

1. Add CNAME file with your domain
2. Configure DNS pointing to GitHub Pages
3. Enable HTTPS in repository settings

### Self-Hosted

```bash
# Simply copy index.html to your web server
cp index.html /var/www/html/onboarding/
```

## API Endpoints (When Connected)

### Submit Patient Data

```
POST /api/patient-onboarding
Content-Type: application/json

{
  "practitioner": "dr_inu",
  "firstName": "John",
  "lastName": "Doe",
  "email": "john@example.com",
  "phone": "+27123456789",
  "appointmentDate": "2026-05-01",
  "medicalHistory": { ... },
  "consents": { ... }
}
```

## Troubleshooting

### Voice Call Not Opening

1. Check pop-up blocker settings
2. Verify ElevenLabs agent ID is correct
3. Ensure internet connection is active

### Form Not Submitting

1. Check browser console for errors
2. Verify all required fields are filled
3. Check localStorage availability

### Mobile Layout Issues

1. Clear browser cache
2. Check viewport meta tag
3. Test in incognito mode

## Support & Maintenance

For issues or feature requests, please:
1. Check the troubleshooting section
2. Review browser console for errors
3. Contact the development team

## License

Proprietaryto Winelands Periodontics. All rights reserved.

---

**Last Updated:** April 2026
**Version:** 1.0.0
**Status:** Production Ready ✅
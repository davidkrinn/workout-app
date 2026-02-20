# Complete Step-by-Step Implementation Guide

## Step 1: Get Your OpenWeatherMap API Key

1. Go to https://openweathermap.org/
2. Click **"Sign In"** (top right)
3. Click **"Create an Account"**
4. Fill out the registration form and submit
5. Check your email and click the verification link
6. Once logged in, go to https://home.openweathermap.org/api_keys
7. Copy your API key (it looks like: `a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6`)
8. ⏱️ Wait 10-15 minutes for the key to activate

---

## Step 2: Locate Your Website Files

1. Open your computer and find your website project folder
2. You should see your HTML file (probably `index.html` or similar)
3. Open this file in a text editor (VS Code, Sublime Text, Notepad++, etc.)

---

## Step 3: Add the Workout Recommendation Code

### Find where your weather widget is in your HTML

Look for this section (or similar):
```html
<a class="weatherwidget-io" href="..." data-label_1="SEATTLE"...>
```

### Right after the weather widget, add this code:

```html
<!-- Workout recommendation container -->
<div id="workout-recommendation">
  <p id="workout-text">Checking weather...</p>
</div>

<script>
  const API_KEY = 'PASTE_YOUR_API_KEY_HERE'; // Replace with your actual API key
  const CITY = 'Seattle'; // Change to your city name

  async function getWorkoutRecommendation() {
    try {
      const response = await fetch(
        `https://api.openweathermap.org/data/2.5/weather?q=${CITY}&appid=${API_KEY}&units=imperial`
      );
      const data = await response.json();
      
      const temp = data.main.temp;
      const weatherCondition = data.weather[0].main.toLowerCase();
      
      let recommendation = '';
      let isIndoor = false;
      
      // Check for rain or snow first (override)
      if (weatherCondition.includes('rain') || 
          weatherCondition.includes('snow') || 
          weatherCondition.includes('drizzle')) {
        recommendation = '🏠 Good day for indoor workout';
        isIndoor = true;
      }
      // Check temperature range
      else if (temp < 50 || temp > 90) {
        recommendation = '🏠 Good day for indoor workout';
        isIndoor = true;
      }
      else {
        recommendation = '🌤️ Good day for outdoor workout';
        isIndoor = false;
      }
      
      const workoutText = document.getElementById('workout-text');
      workoutText.textContent = recommendation;
      
      const container = document.getElementById('workout-recommendation');
      container.className = isIndoor ? 'indoor' : 'outdoor';
      
    } catch (error) {
      console.error('Error fetching weather:', error);
      document.getElementById('workout-text').textContent = 'Unable to load recommendation';
    }
  }
  
  getWorkoutRecommendation();
  setInterval(getWorkoutRecommendation, 30 * 60 * 1000);
</script>
```

### Important: Replace these values:
- Replace `PASTE_YOUR_API_KEY_HERE` with your actual API key
- Replace `Seattle` with your city name

---

## Step 4: Add the CSS Styling

Find the `<style>` section in your HTML (usually in the `<head>` section), or if you have a separate CSS file, open that.

Add this CSS code:

```css
#workout-recommendation {
  max-width: 300px;
  margin: 15px auto 0;
  padding: 15px;
  border-radius: 8px;
  text-align: center;
  font-weight: 600;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  transition: all 0.3s ease;
}

#workout-recommendation.outdoor {
  background: linear-gradient(135deg, #84fab0 0%, #8fd3f4 100%);
  color: #1a5f3f;
  border: 2px solid #5cb85c;
}

#workout-recommendation.indoor {
  background: linear-gradient(135deg, #fbc2eb 0%, #a6c1ee 100%);
  color: #4a4a4a;
  border: 2px solid #d9534f;
}

#workout-text {
  margin: 0;
  font-size: 16px;
}
```

---

## Step 5: Test Locally (Optional but Recommended)

1. Save your HTML file
2. Double-click the HTML file to open it in your browser
3. You should see:
   - Your weather widget
   - Below it, a colored box with workout recommendation
4. Open the browser console (F12 or right-click → Inspect → Console tab)
5. Check for any errors

---

## Step 6: Deploy to Vercel

### If you're using Git (most common):

1. Open your terminal/command prompt
2. Navigate to your project folder:
   ```bash
   cd path/to/your/project
   ```

3. Add your changes:
   ```bash
   git add .
   ```

4. Commit your changes:
   ```bash
   git commit -m "Add workout recommendation widget"
   ```

5. Push to your repository:
   ```bash
   git push
   ```

6. Vercel will automatically detect the changes and redeploy (takes 1-2 minutes)

### If you're NOT using Git:

1. Go to https://vercel.com/
2. Log in to your account
3. Find your project
4. Click on it
5. Go to the **Settings** tab
6. Scroll down and click **"Redeploy"** or upload your new files

---

## Step 7: Verify It's Working

1. Go to your Vercel website URL (something like `yoursite.vercel.app`)
2. You should see:
   - The weather widget at the top
   - The workout recommendation below it
   - The recommendation should be green (outdoor) or pink/purple (indoor)

---

## Troubleshooting

### If you see "Checking weather..." and it never changes:

1. **Check your API key**: Make sure you copied it correctly
2. **Wait for activation**: New API keys take 10-15 minutes to activate
3. **Check the console**: Press F12, go to Console tab, look for error messages
4. **Check your city name**: Make sure it's spelled correctly

### If you see "Unable to load recommendation":

1. Open browser console (F12)
2. Look for red error messages
3. Common issues:
   - API key not activated yet
   - City name misspelled
   - API key not pasted correctly

### Test your API key manually:

Visit this URL in your browser (replace with your actual key and city):
```
https://api.openweathermap.org/data/2.5/weather?q=Seattle&appid=YOUR_API_KEY&units=imperial
```

If it works, you'll see weather data. If not, you'll see an error message.

---

## Need Help?

Let me know:
- What step you're on
- Any error messages you see
- Screenshots if helpful

I'm here to help you get this working! 🎉
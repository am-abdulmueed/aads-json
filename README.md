# Ad Dialog Configuration Guide

## Overview

This document is a complete guide for the app’s ad dialog system. In this system, you can fetch JSON config from GitHub and display dynamic ads.

## File Locations

| File          | Path                                                                | Purpose                                       |
| ------------- | ------------------------------------------------------------------- | --------------------------------------------- |
| Main Activity | `app/src/main/java/com/lagradost/cloudstream3/MainActivity.kt:1509` | `showDialogAd()` function - complete ad logic |
| Dialog Layout | `app/src/main/res/layout/dialog_ad.xml`                             | Ad dialog UI layout                           |
| JSON Config   | `https://cdn.jsdelivr.net/gh/am-abdulmueed/ads-json@main/ads.json`  | Online config file                            |

## JSON Configuration

### Complete JSON Structure

```json
{
  "dialog_ad": {
    "enabled": true,
    "badge": "Sponsored",
    "title": "Special Offer!",
    "message": "Check out this amazing opportunity for you.",
    "button_text": "SignUp",
    "image_url": "https://i.ibb.co/kgt1Sj4Y/600x600.webp",
    "click_url": "https://members.ogads.com/register?r=401055",
    "show_after_seconds": 3,
    "auto_close_seconds": 15,
    "start_date": "2026-05-01",
    "end_date": "2026-12-31",
    "max_daily_views": 3,
    "interval_hours": 7
  }
}
```

### Field Descriptions

| Field                | Type    | Required | Description                                                                    |
| -------------------- | ------- | -------- | ------------------------------------------------------------------------------ |
| `enabled`            | Boolean | Yes      | Enable/disable the ad                                                          |
| `badge`              | String  | No       | Badge shown in the top-left corner (e.g., "Sponsored", "Hot Offer")            |
| `title`              | String  | Yes      | Ad title                                                                       |
| `message`            | String  | Yes      | Ad description                                                                 |
| `button_text`        | String  | Yes      | Text displayed on the button                                                   |
| `image_url`          | String  | Yes      | URL of the ad image                                                            |
| `click_url`          | String  | Yes      | URL opened when the button is clicked                                          |
| `show_after_seconds` | Integer | Yes      | How many seconds after app launch the ad will show                             |
| `auto_close_seconds` | Integer | Yes      | After how many seconds the countdown ends and the close button appears         |
| `start_date`         | String  | Yes      | When the ad starts showing (Format: YYYY-MM-DD)                                |
| `end_date`           | String  | Yes      | When the ad stops showing (Format: YYYY-MM-DD)                                 |
| `max_daily_views`    | Integer | Yes      | Maximum number of times the ad can show per day                                |
| `interval_hours`     | Integer | Yes      | Minimum gap in hours between two ad displays                                   |

## Dialog UI Flow

1. **Dialog Appears**
   - Countdown timer shows in the top-right corner
   - Badge (if provided) shows in the top-left corner
   - Order: Badge → Title → Image → Description → Button
2. **After Countdown Ends**
   - Timer disappears
   - Close (X) icon appears
   - User can dismiss the dialog by clicking the close icon

## Shared Preferences Keys

These keys are used to store data:

| Key                        | Purpose                                      |
| -------------------------- | -------------------------------------------- |
| `LAST_DIALOG_AD_SHOW_TIME` | Last time the ad was shown (milliseconds)    |
| `DIALOG_AD_VIEWS_TODAY`    | Number of times the ad has shown today       |
| `DIALOG_AD_LAST_VIEW_DATE` | Last date the ad was shown                   |

## Automatic Badge Color System

Badge color is automatically set based on the text. No need to manually assign colors!

| Badge Text     | Background Color | Text Color | Meaning                 |
| -------------- | ---------------- | ---------- | ----------------------- |
| `Sponsored`    | Blue (#1E88E5)   | White      | Trust & Professionalism |
| `Promotion`    | Green (#43A047)  | White      | Growth & Deals          |
| `Hot Offer`    | Orange (#FB8C00) | White      | Urgency & Limited Time  |
| `Hot`          | Orange (#FB8C00) | White      | Urgency & Limited Time  |
| `Exclusive`    | Purple (#8E24AA) | White      | Premium & Luxury        |
| `Premium`      | Gold (#FFD700)   | Black      | High-Value Offers       |
| `VIP`          | Gold (#FFD700)   | Black      | High-Value Offers       |
| Any other text | Red (#E53935)    | White      | Default fallback        |

### Example JSON Configs

**Sponsored Ad:**
```json
{
  "badge": "Sponsored",
  "title": "Special Offer!"
}
```

**Hot Offer:**
```json
{
  "badge": "Hot Offer",
  "title": "Limited Time Deal!"
}
```

**Promotion:**
```json
{
  "badge": "Promotion",
  "title": "Special Promotion!"
}
```

**Exclusive Deal:**
```json
{
  "badge": "Exclusive",
  "title": "Exclusive Offer!"
}
```

## Customization Tips

### Available Badge Drawables

If you want to manually change badge backgrounds, these drawables are available:

| Drawable File         | Color  |
| --------------------- | ------ |
| `badge_bg_blue.xml`   | Blue   |
| `badge_bg_gray.xml`   | Gray   |
| `badge_bg_green.xml`  | Green  |
| `badge_bg_orange.xml` | Orange |
| `badge_bg_red.xml`    | Red    |
| `badge_bg_purple.xml` | Purple |
| `badge_bg_gold.xml`   | Gold   |

### Change Image Radius

To round image corners, edit `app/src/main/res/drawable/rounded_image.xml`:

```xml
<corners android:radius="24dp" />  <!-- Change this value -->
```

### Change Button Style

The app already has a `WhiteButton` style defined. It’s already applied in the layout.

## Example Badge Ideas

| Badge Text  | Color            | Use Case             |
| ----------- | ---------------- | -------------------- |
| `Sponsored` | Blue (#1E88E5)   | Normal sponsored ads |
| `Hot Offer` | Orange (#FB8C00) | Limited time offers  |
| `Promotion` | Green (#43A047)  | Special promotions   |
| `Exclusive` | Purple (#8E24AA) | Exclusive deals      |
| `Premium`   | Gold (#FFD700)   | Premium offers       |

## Important Notes

1. **Required JSON Fields**: `title`, `message`, `button_text`, `image_url`, `click_url` must not be empty, otherwise the ad won’t show  
2. **Date Format**: Always use `YYYY-MM-DD` for `start_date` and `end_date`  
3. **Time Calculation**: All time calculations are based on the local device time  
4. **No Auto-Dismiss**: After countdown ends, the dialog won’t auto-dismiss. Only the close button will appear  

---
**Licensed under the MIT License © 2026 Abdul Mueed**

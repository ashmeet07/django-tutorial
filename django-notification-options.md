
# All notification options present in Desktop/Mobile/Web application project !
*lets explore each notification category what we have for application*


## Types of notificaiton
- In-App notification
- Real-Time notification
- Push Notification
- Email Notification
- SMS notification
- Desktop notification
- Mobile notification
- System notification
- Event - Based notification
- Background notification


##  In-App notification
In this notification it basically reside inside the app like having notification on chat icon in instagram, notification badge on teams chat or activity.

*Used in all application having internal ui*

`User Action → Django View/API → Notification Model → Database → Frontend Fetch → Notification UI`

## Real-Time notification
Appear instantly without page refresh whatsapp live message, slack updates, alerts, traiding alerts, live dashboard.

*Used in all application having live updates feature*

`User Action → Django Backend → Channels Consumer → WebSocket → Browser Receives Instantly → Live UI Update`

## Push notification
Notification delivered even application is closed category comes in active notification.

*Used in all apps like update and so on for sensitive app where update is necessary*

`Django Backend → Firebase/Push Service → Service Worker → Browser → System Notification Popup`

## Email notification
When user want a formal communication then we use email notification like forget password, invoice, otp-mail, welcome-mail.

*Used in all apps but for specific purpose*

`Django Event → Email Service → SMTP Provider → User Inbox`

## SMS notification
It is a medium for otp-related tasks and for general updates mostly used-in-banking-system.

*Used in all apps but for general updates regarding internal detials mostly*

`Django Backend → SMS API (Twilio/MSG91) → Telecom Network → User Phone`

## Desktop notification 
Only used in desktop applications it works like like pop-up on desktop happen like firewall.

*Used in desktop-application like: at the time of installation giving firewall permission, or like opening app in administrative privilage*

## System notification
Os-level notification system update, antivirus alert, system warnings.

*Used on os-level to provide alerts to the applications or the user*

## Even-Driven notification
Trigger only when an event occur.

*Used to provide update to user after trigging an event or job*

## Background notification
Trigger while app running on background like in iphone background apps also provide notification while app is running the process.

*Used all devices becasue if device supported the background process to run like in multi-process env so each app gives notification update if have any*


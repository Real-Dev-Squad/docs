# Analytics

Tool we'll be using: [Mixpanel](https://mixpanel.com/)

## Things we'll be tracking:
- Event name (example, user is on signup page)
- Additional data related to that particular event name (example: which step of signup is the user on)
- User related data (example: looking for job/not, student/not, gender)

## Things we'll NOT be tracking:
- PII (Personal Identifiable Information) like first/last name, username, etc

## How will we track events for a user ?
- We'll hash the user's `userId` (not `username`) and then track the user through that hash

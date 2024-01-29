# 🚀 Book 6 - Capstone Project

| Question | Where to look |
|---|---|
| What are the requirements for my proposal? | It's on the [Learning Platform](https://learning.nss.team/) |
| How should I build my user stories? | Refer to the tickets in [Deshawn's](../book-2-web-apis/chapters/deshawns-user-stories.md) |
| How should I set up my project and directories? | The HouseRules [setup chapter](../book-4-controllers/chapters/house-rules-setup.md) shows you exactly how to do it. |
| How should I build my project README | It's on the [Learning Platform](https://learning.nss.team/) |
| How do I know who the authenticated user is? | `App.jsx` has a state variable called `loggedInUser` that should have a value when a user is logged in. Pass that variable as a prop down to any component that needs it. If it is impractical to pass that prop down into many nested components (prop drilling), a component can use the `tryGetLoggedInUser` function in the `authManager` module to retrieve the user associated with the login cookie from the API.|
| How often should I push to Github? | <ul><li>Each time you get something working</li><li>Each time you complete the code on a feature branch</li><li>If you feel anxious that it's been a while since your last push</li></ul>  |
| Can I work on my `main` branch since it's my capstone? | No. Never. We'll find you. **Always** work on a feature branch. |
| Should my ERD table names be plural? | No, stick with singular to conform with the C# classes you are using to build the database with EF Core. |

## Deploying Your Applications

The following chapters will walk you through how to deploy your client applications and API applications.

| # | ⛅️ Deployment  |
|--|--|
| 1 | [Digital Ocean Deploy](./chapters/deployment-digital-ocean.md) |
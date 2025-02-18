# **📨 godot-itch-deploy**
Easily deploy your web games to itch.io using this GitHub Action! 

[Heavily based from a solution found on Y Combinator.](https://news.ycombinator.com/item?id=36466698)

---
Ahead are instructions to get this Workflow running inside your game's repo, written **as if it's your first time using Actions.** Skip ahead if you know what you're doing.

✉️ If something doesn't work, there's an FAQ below. If I messed something up, **send a pull request and I'll sort it out.** Thanks!

---

## **Instructions**
### **🧑‍🍳 Before proceeding:**
- I'm assuming you're using GitHub for your version control (otherwise, you wouldn't be here anyways 😅).
- I'm assuming you're using **Godot 4.3-stable downloaded from the website.**
  - This may not work on Steam or itch.io versions of the engine. Don't quote me on that.

### 📦 Set-up
You might want to check the 🙋‍♂️ FAQ below before starting. It has valuable information and context you might want to know.

#### 💻 Installation
**Creating your itch.io game page.**
- Create a new game just like any other.
  - ⚠️ Make sure your visibility and access is set to either **Draft** (if you're working alone) or **Restricted** (if you're working with a team, so people you choose can access the game page).
- Write down your username. It's the text behind the `.itch.io` link that goes to your profile page.
  - In my case, it's `framebuffers`.
  - ![image](https://github.com/user-attachments/assets/adb82977-9d6b-44cd-bb56-17a0cda9df61)

---

**Getting an itch.io API key.**
- Get your [itch.io API key](https://itch.io/user/settings/api-keys) here. 
  - You'll go to a page like this one. ![image](https://github.com/user-attachments/assets/6ee5d61c-bdd2-477a-8bab-e6c272b2cbb3)
  - Click on **Generate new API key.**
- You'll get a message saying **API key created**. Below that, You'll have a table with your API key, Source and Creation date.
  - Click on **View** and copy that string. ![image](https://github.com/user-attachments/assets/508fd5e0-8c4f-4537-8906-89ea761a3c17)
  - ⚠️ Don't share this key! It gives you to control over your account through the API.
  - ℹ️ If you want to revoke it, click on **Revoke.**

---

ℹ️ Before proceeding, make sure you have:
- ✅ Your itch.io username.
- ✅ Your game name.
- ✅ Your itch.io API key.

---

**Creating the secrets**
- Inside the GitHub page of your game, click on Settings. ![image](https://github.com/user-attachments/assets/0dd1bd72-76a4-4d8d-aff4-c0a9adb92110)
- On the left panel, inside the Security section, click **Secrets and variables** and then **Actions.** ![image](https://github.com/user-attachments/assets/73bc70b4-51fd-456c-8244-b5f62f0403f5)
- Click on **New repository secret.**
- Create three secrets:
  - `ITCHAPI`: your itch.io API key,
  - `USERNAME`: your itch.io username,
  - `GAMENAME`: your game name.
- You should have something like this: ![image](https://github.com/user-attachments/assets/4cb2ed63-da20-4261-9a81-970778771f09)
  - 🎉 Now you're ready to deploy!

---

**Preparing your project folder**
- Create a folder called `.github` inside the root of your project. Inside that new folder, create another folder called `workflows`.
- Place `main.yaml` inside `workflows`.
- Create a commit to the repository you chose to run the Action on. By default, the Workflow is configured to look for a `main` branch.
- Push it to GitHub!

---

## 📤 Deployment
Your game will go through a `Workflow`. What this does, is create a container with everything needed to compile your game, package it into a ZIP file and upload it to itch.io
Every single time you commit to the chosen branch, a **workflow will run over your committed codebase.**
The first time **may take a while**, up to 10 minutes in some cases, because GitHub must get all the necessary files from scratch.
**Later runs will be much faster (down to 2 minutes or so).**
Commits **outside** the monitored branch will **skip** the check:
![image](https://github.com/user-attachments/assets/b7255db7-3d26-4546-84e0-83c3bebd0246)

Inside each run, you'll find the details of each step taken to get your build up and running. If you add or delete any steps, you'll see them reflected here.
![image](https://github.com/user-attachments/assets/c2659979-6a78-4bdf-823c-fc25e66f660f)

If all goes well, there should be a game ready to play!
![image](https://github.com/user-attachments/assets/7a572451-87ae-4085-8f55-a5ca3ba4f88f)

---

# 🙋‍♂️ FAQ:
- **My game doesn't appear on itch/it shows an older build!**
  - Make sure this item is checked inside your game's Dashboard (where you can change the name and such) ![image](https://github.com/user-attachments/assets/67fbbd69-ab5f-48da-93a9-dc5197bdf947)
- **What are 'secrets'?**
  - This one was confusing to me at first, but let me explain: 
    - At the end of `main.yaml`, you'll see these weird strings: ![image](https://github.com/user-attachments/assets/687773b7-d0e1-4c25-9524-2d9a07ce369e)
      - These are environment variables that have sensitive data, like API keys, needed to run this Workflow. These variables will be defined on the _Actions secrets and variables_ window.
      - `secrets.ITCHAPI` means that it'll look for the variable named `ITCHAPI` inside the `secrets` tab above the keys.
- **My workflow is failing in the middle!**
  - There are several reasons why:
    - Make sure you have **every single asset** you need to compile your game inside the the project folder.
      - If your game is large, use this Workflow at your own discretion.
    - You may need to compile an initial web build before proceeding.
      - This step is done so you generate a `export_presets.cfg` file.
      - ℹ️ What I did, is compiling inside a folder called `bin`, and then adding it to my `.gitignore`.
    - Check your `.gitignore` before proceeding.
      - You don't want any large binaries or personal data inside your GitHub repo, even if it's private.
    - You may have issues with your game name.
      - ⚠️ If, by chance, uses any spaces or special characters, you'll need to write them down **inside single quote marks (`'`).**
      - ℹ️ This depends a lot on a case-by-case basis, but if you have problems with this later, just hardcode the name of your game on the YAML file. You can do the same with the game name... **just don't do it with the API key.**
- **Why do I need to create the itch.io page if I haven't finished my game?**
  - You need to if you're running this anyway, but I **don't recommend deploying _that_ often.**
  - ℹ️ For game jams (and other cases) it is good practice to have your itch.io page ready to go. It's time consuming, and you **don't want to do this at the last minute, especially under pressure.**
  - ℹ️ I'd recommend you create a separate branch to work on smaller changes instead of working on your main branch only. This way, you don't trigger this action _every time you commit._
- **What does "billable time" mean? Are they gonna charge me money for this?**
  - ![image](https://github.com/user-attachments/assets/2be65625-7473-41bd-b5b2-3895e2fd5d66)
  - This also caught me off-guard. Turns out that each account has an allowance to run these Actions.
  - [Click here for more information about this that I found.](https://michaelcurrin.github.io/dev-cheatsheets/cheatsheets/ci-cd/github-actions/usage-limits.html)
  - ℹ️ **TL;DR: don't worry, it should be free.** It'll error out if your Workflow is too spicy and you need to pay for it.
 
---

btw thanks to [Bitlytic](https://github.com/Bitlytic) for showing me this ❤️, check em out in [YouTube](https://www.youtube.com/@Bitlytic) for good Godot goodness.

-frame

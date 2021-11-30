# Important boilerplate files

## All important files :

    📦blank_boilerplate
     ┣ 📂api
     ┃ ┣ 📂project
     ┃ ┃ ┣ 📜settings.py    //-> your backend apps and middleware here
     ┃ ┃ ┣ 📜urls.py        //-> your backend new routes here
     ┃ ┗ 📜requirements.txt //-> your backend libs here
     ┣ 📂app
     ┃ ┣ 📂public
     ┃ ┃ ┣ 📜[...]          //-> custom icons / app name / embbed js libs
     ┃ ┣ 📂src
     ┃ ┃ ┣ 📜i18n.ts        //-> your translations
     ┃ ┃ ┣ 📜index.tsx      //-> call frontend Munity framework
     ┃ ┃ ┣ 📜store.ts       //-> your frontend store
     ┃ ┃ ┗ 📜styles.scss    //-> your frontend style
     ┃ ┣ 📜package.json     //-> your frontend libraries
     ┣ 📜.env               //-> your platform settings
     ┣ 📜docker-compose.yml //-> your services

## Frontend, the src/index.ts file

The is the root of your frontend application.
Munity is invoked here. First Munity Proverds, then Munity main component. This main component take many params to tweak default behavior :

```jsx
<MunityApp
    // Cosmetic components
    logoLogin={logo}
    loadingWorkspace={LoadingMunity}

    // Functionnal components
    overmindSidebar={<MunityOvermindSidebar />}
    workspaceNavbar={<Navbar
        leftPart={NavbarLeft}
        centerPart={NavbarCenter}
        rightPart={NavbarRight}
    />}
    overmindNavbar={<OvermindNavbar
        leftPart={OvermindNavbarLeft}
        centerPart={OvermindNavbarCenter}
        rightPart={OvermindNavbarRight}
    />}

    // Routes
    newOvermindRoutes={[
        // <Route key={'overmind_youpibanana'} path="/youpibanana" component={() => <>Overmind Youpi Banana \o/</>} />
    ]}
    newWorkspaceRoutes={[
        // <Route key={'workspace_youpibanana'} path="/workspace/:workspace_slug/youpibanana" component={() => <>Workspace Youpi Banana \o/</>} />
    ]}
>
    {/* Custom routes outsite of overmind or workspace, usefull for fullscreen pages*/}
    {/* <Route path="/maps/:location/only_active" component={((location: string) => MapFullscreen({ location }))} /> */}
</MunityApp>
```

## Backend, the projet/settings.py file

This is main configuraiton file for your application. Munity is already bootstrapped inside and you can extend as a standard Django Rest Framework application behavior.

This file is pretty big so we only show interesting parts :

```py
# [...]

# Application definition
INSTALLED_APPS = [
    # Munity is load here
    "munity",
    "munity.generic_groups",
    "munity.users",
    "munity.workspaces",
    "munity.authorization",
    "munity.records",
    "munity.settings",
    # App
    # Add your applications here (ex: "myapp" will refear ./api/myapp directory)
    # See this documentation to create your apps : https://docs.djangoproject.com/en/3.2/intro/tutorial01/#creating-the-polls-app
]

# [...]

MIDDLEWARE = [
    # [...]
    # Add your middleware here
]

# [...]
#  If you extend user model you can change the default model user here
AUTH_USER_MODEL = "users.User"

```
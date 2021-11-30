# Customize munity

Please see the [manage custom content](managecustomcontent.md) to know how to add custom content.

## Improve logo, navbars, sidebar and loader

You can write your own navbar and inject its directly on MunityApp component has below :

```typescript
<MunityApp
    logoLogin={logo}
    overmindNavbar={
        <OvermindNavbar
            leftPart={OvermindNavbarLeft}
            centerPart={() => <div />}
            rightPart={OvermindNavbarRight}
        />
    }
    workspaceNavbar={<Navbar
        leftPart={WorkspaceNavbarLeft}
        centerPart={() => <div/>}
        rightPart={NavbarRight}
    />}
    overmindSidebar={<OvermindSidebar />}
    loadingWorkspace={() =>
        <div className="app-loading">
            <img src={logo} alt="Logo" />
            <ProgressSpinner />
        </div>
    }
>
```

## Change layout

1\. You can change CSS variables in your CSS file.

```css
@charset "UTF-8";
:root {
    --surface-a:#ffffff;
    --surface-b:#faf9f8;
    --surface-c:#f3f2f1;
    --surface-d:#edebe9;
    --text-color:#323130;
    --primary-color:rgb(250, 170, 65);
    --primary-color-text:#333;
    --secondary-color: rgb(197, 122, 23);
    --secondary-color-text:#605e5c;
    --content-padding:1rem;
    --inline-spacing:0.5rem;
    --error-color: #ef9a9a;
    --valid-color: #54b358;
    --box-shadow: #777;
}
```

2\. You can also generate a full template from primereact, we are 100% compatible

[PRIME DESIGNER](https://www.primefaces.org/designer-react/#/)


3\. Finally feel free to override what you want in your CSS.


## How to extend current user

By default the user used by Munity backend is from `./projects/settings.py` :

```py
# [...]
INSTALLED_APPS = [
    "munity.users",
    # [...]
]
# [...]
AUTH_USER_MODEL = "users.User"
# [...]
```

You can if you want extend the user model with a 1 - 1 relation (Proxy Model). Django explain this [here](https://docs.djangoproject.com/en/3.2/topics/db/models/#proxy-models).

To so you have to extend user model :

```py
from munity.users.models import User

class AppUser(User):
    role = models.CharField(max_length=30)
    age = models.CharField(max_length=30)
    computer = models.ForeignKey(Computer, related_name='computer_user', on_delete=SET_NULL, blank=True, null=True)
```

Then adapt your `project/settings.py` :

```py
# [...]
INSTALLED_APPS = [
    "munity.users",
    # [...]
]
# [...]
AUTH_USER_MODEL = "appuser.AppUser"
# [...]
```

Since a User model is linked to an AppUser, application works as expected with an extended user.

## How to create group categories

We recommand to use [Proxy Model](https://docs.djangoproject.com/en/3.2/topics/db/models/#proxy-models) also to create your own categories and keep `Generic Group system`.

For exemple you can create a `Building` group and a `TicketType` group then assign its independently to a `Ticket` mode :

**On buildings/models.py**
```py
from munity.generic_groups.models import GenericGroup
class Building(GenericGroup):
    total_floor = models.PositiveIntegerField(default=0)
    address = models.CharField(max_length=256)
```

**On tickettypes/models.py**
```py
from munity.generic_groups.models import GenericGroup
class TicketType(GenericGroup):
    type = models.CharField(max_length=256)
```

**On ticket/models.py**
```py
class Ticket(MunityModel):
    #[...]
    building = models.ForeignKey("buildings.Building", related_name='building_tickets', on_delete=SET_NULL, blank=True, null=True)
    type = models.ForeignKey("tickettypes.TicketType", related_name='type_tickets', on_delete=SET_NULL, blank=True, null=True)
```

You can now use specific group on your `Ticket` model without creating custom tables.
# Hello World static web app

A dependency-free site ready for Azure Static Web Apps.

## Run locally

Open `index.html` in a browser, or serve the directory with any static file server.

For example, if Python is installed:

```powershell
python -m http.server 8000
```

Then open <http://localhost:8000>.

## Deploy to Azure Static Web Apps

1. Push this directory to a GitHub or Azure DevOps repository.
2. Create an **Azure Static Web App** resource in the Azure portal.
3. Connect the resource to the repository and use these build settings:

   | Setting | Value |
   | --- | --- |
   | App location | `/` |
   | API location | Leave empty |
   | Output location | Leave empty |

Azure creates a deployment workflow in the repository. After that workflow completes, open the URL shown on the Static Web App resource's overview page.

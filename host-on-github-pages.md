## Host on GitHub Pages
<p class='muted'>Here are the deployment steps, assuming you already have a Blazor WebAssembly project in your GitHub repository. This guide focuses on the deployment process:</p>

To automatically deploy your Blazor WebAssembly app to GitHub Pages whenever you push code to your repository, you can set up a **GitHub Actions workflow**. This way, the publishing and deployment process happens automatically. Below is a step-by-step guide on how to set this up:

### Step 1: Modify the Base Href in `index.html`
1. In your Blazor WebAssembly project, navigate to the `wwwroot/index.html` file.
2. Modify the `<base>` tag to reflect your GitHub repository name. For example, if your repository is named `repository-name`, set the base href like this:

   ```html
   <base href="/repository-name/">
   ```

### Step 2: Create a GitHub Actions Workflow for Deployment

1. **Go to your GitHub repository** where the Blazor WebAssembly app is located.
2. **Create a `.github/workflows` directory** if it doesn’t already exist. Inside this folder, create a file called `deploy.yml` (or any name you prefer for your workflow file).

   The directory structure will look like this:
   ```
   .github/
     └── workflows/
         └── deploy.yml
   ```

3. **Add the following content to `deploy.yml`**:

   ```yaml
   name: Deploy Blazor WebAssembly App to GitHub Pages

   on:
     push:
       branches:
         - main  # Trigger the workflow only when pushing to the main branch

   jobs:
     build:
       runs-on: ubuntu-latest

       steps:
         # Step 1: Checkout the code
         - name: Checkout code
           uses: actions/checkout@v3

         # Step 2: Set up .NET SDK
         - name: Set up .NET
           uses: actions/setup-dotnet@v3
           with:
             dotnet-version: '6.0'  # Replace with the .NET version you're using

         # Step 3: Restore dependencies
         - name: Restore dependencies
           run: dotnet restore

         # Step 4: Publish the Blazor app to the `publish` directory
         - name: Build and Publish Blazor WebAssembly app
           run: |
             dotnet publish -c Release -o ./publish

         # Step 5: Deploy to GitHub Pages
         - name: Deploy to GitHub Pages
           uses: JamesIves/github-pages-deploy-action@v4
           with:
             branch: gh-pages  # Deploy to the gh-pages branch
             folder: publish   # The folder to deploy (containing the Blazor app's build)
             clean: true        # Clean the `gh-pages` branch before deployment
   ```

### Step 3: Configure GitHub Pages

1. **Go to the Settings of your repository**.
2. Scroll down to the **GitHub Pages** section.
3. Under **Source**, select the `gh-pages` branch.
4. Save the changes.

### Step 4: Push Code to GitHub

Now, whenever you push your changes to the `main` branch of your repository, the GitHub Actions workflow will trigger and automatically:

- Build and publish your Blazor WebAssembly app.
- Deploy the contents of the `publish` folder to the `gh-pages` branch of your repository.

### Step 5: Access Your App

Once the workflow runs successfully, your app will be available at:

```
https://username.github.io/repository-name/
```

If you're using a custom domain, make sure to set that up in the **GitHub Pages** section of the repository's settings as mentioned earlier.

### Step 6: (Optional) Custom Domain

If you want to use a custom domain, follow these steps:
1. Add a `CNAME` file in the `publish` folder with the custom domain inside it (e.g., `www.yourdomain.com`).
2. Update your domain registrar to point to GitHub Pages (`username.github.io`).
3. In your GitHub repository, add the custom domain in the **GitHub Pages** section of the settings.

### Conclusion

Now, every time you push changes to the `main` branch, GitHub Actions will automatically build and deploy your Blazor WebAssembly app to GitHub Pages. You can monitor the deployment progress in the **Actions** tab of your GitHub repository.

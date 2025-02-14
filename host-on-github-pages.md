## Host on GitHub Pages
Here are the deployment steps, assuming you already have a Blazor WebAssembly project in your GitHub repository. This guide focuses on the deployment process:

### Step 1: Modify the Base Href in `index.html`
1. In your Blazor WebAssembly project, navigate to the `wwwroot/index.html` file.
2. Modify the `<base>` tag to reflect your GitHub repository name. If your repository is named `repository-name`, change the `<base>` tag like this:

   ```html
   <base href="/repository-name/">
   ```

   This ensures that the app correctly handles paths when hosted on GitHub Pages.

### Step 2: Publish the Blazor WebAssembly App
1. **Open a terminal** in your project’s root directory (where your `.csproj` file is).
2. Run the following command to publish your app for production:

   ```bash
   dotnet publish -c Release -o ./bin/Release/net5.0/publish
   ```

   This command creates a `publish` folder containing all the necessary static files (HTML, CSS, JS, etc.) that need to be hosted on GitHub Pages.

### Step 3: Push the Published Files to GitHub
1. **Navigate to your repository** folder where the `publish` folder was created (if you're not already in that directory).
2. Copy all the contents of the `bin/Release/net5.0/publish` directory into the root of your GitHub repository (or into a folder like `docs` if you want to use that for GitHub Pages).

   ```bash
   cp -r ./bin/Release/net5.0/publish/* /path/to/your/repository
   ```

3. **Commit and push the changes** to your GitHub repository:

   ```bash
   git add .
   git commit -m "Deploy Blazor WebAssembly app to GitHub Pages"
   git push origin main
   ```

### Step 4: Set up GitHub Pages
1. **Go to your GitHub repository** and click on the **Settings** tab.
2. Scroll down to the **GitHub Pages** section.
3. Under **Source**, select either the root (`/`) or the `docs/` folder (if you chose to use the `docs/` folder).
4. Click **Save**.

Your Blazor app will be deployed to GitHub Pages at:

```
https://username.github.io/repository-name/
```

### Step 5: (Optional) Use a Custom Domain
If you want to use a custom domain with GitHub Pages, follow these steps:

1. In the **GitHub Pages** section of your repository’s settings, enter your custom domain (e.g., `www.yourdomain.com`).
2. Update your domain's DNS settings to point to GitHub Pages by creating a **CNAME** record that points to `username.github.io`.
3. Create a `CNAME` file in your repository and add your custom domain (e.g., `www.yourdomain.com`) inside it.

### Step 6: Verify the Deployment
1. Go to the URL `https://username.github.io/repository-name/` (or your custom domain) to see your Blazor WebAssembly app live on GitHub Pages.
2. If there are any issues, check your browser’s console for errors related to loading resources or check the repository settings.

That’s it! Your Blazor WebAssembly app is now deployed on GitHub Pages.

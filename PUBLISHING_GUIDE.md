# Publish this portfolio to GitHub

1. Open this package and verify its three notebooks execute locally with the original source CSV.
2. Sign in to GitHub and create an empty public repository named `orange-belgium-churn-retention-analytics` (do not auto-create a README, license or .gitignore; this package has them).
3. From a terminal in the extracted project root, run:

```bash
git init
git add .
git status
git commit -m "Publish Orange Belgium churn and retention analytics portfolio"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/orange-belgium-churn-retention-analytics.git
git push -u origin main
```

4. Replace `YOUR_USERNAME` with your GitHub username; authenticate using GitHub's supported method. Do not paste credentials into your notebook.
5. Verify `README.md` displays, all three notebook links open, the source CSV was excluded, and notebook outputs are sensible.
6. Add the project to your CV or portfolio with a short description: *Built and evaluated CatBoost churn-risk models (held-out ROC-AUC 0.844; AP 0.198) and compared exploratory T- and S-Learner retention uplift strategies, including uncertainty diagnostics and causal limitations.*

For a GitHub browser-based upload, choose **Add file > Upload files** on the empty repository and upload the contents of the package, not the original CSV. A connected GitHub integration may also permit direct publishing. Never claim publication before confirming a repository URL.

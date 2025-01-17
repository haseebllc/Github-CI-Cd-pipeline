## create in root dir .github/workflows/ci-cd.yaml

```bash
name: CI-CD pipelines to prevent automate deploy on test failure

on:
  push:
    branches:
      # Run Jobs only when code push on these branches
      - main
      - master
  pull_request:
    branches:
      # Run workflows only when making a pull request on these branches
      - main
      - master

jobs:
  # CI { Continuous Integration }
  test:
    name: Run Tests on latest OS
    runs-on: ubuntu-latest #{ Linux in this case }

    steps:
      - name: Checkout latest code in repo
        uses: actions/checkout@v2

      - name: Set up Node.js environment
        uses: actions/setup-node@v2

      - name: Install dependencies
        run: npm install

      - name: Run tests
        run: npm test

  # CD { Continuous Deployment }
  deploy:
    name: Deploy to Vercel
    runs-on: ubuntu-latest # OS for deployment
    needs: test # 'deploy' job will only run if the 'test' job passes

    steps:
      - name: Checkout code from GitHub
        uses: actions/checkout@v2

      - name: Build Project (Optional for Next.js or other build processes)
        run: npm run build # Ensure your project builds without errors

      - name: Deploying to Vercel
        run: npx vercel --prod # Use Vercel CLI to deploy project in production
```

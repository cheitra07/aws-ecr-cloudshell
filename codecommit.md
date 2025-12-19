Perfect! Cheitra, AWS CodeCommit is **AWS’s fully managed Git repository service**. You can use it instead of GitHub to store your CodeBuild source and avoid the CloudShell/S3 workaround. I’ll give you a **step-by-step guide** to get your demo project running.

---

# ✅ Step 1: Create a CodeCommit repository

1. Go to **AWS Console → CodeCommit → Repositories → Create repository**
2. Name: `codebuild-demo`
3. Description (optional): `Demo for CodeBuild Docker image`
4. Click **Create repository**

> You’ll see a URL like:
> `https://git-codecommit.us-east-1.amazonaws.com/v1/repos/codebuild-demo`

---

# ✅ Step 2: Configure Git in CloudShell

CloudShell already has Git installed.

```bash
# Go to your project folder
cd ~/codebuild-demo

# Initialize Git
git init
```

---

# ✅ Step 3: Add CodeCommit as remote

```bash
git remote add origin https://git-codecommit.us-east-1.amazonaws.com/v1/repos/codebuild-demo
```

> Use the HTTPS URL from your CodeCommit repository.

---

# ✅ Step 4: Commit your files

```bash
git add .
git commit -m "Initial commit for CodeBuild demo"
```

---

# ✅ Step 5: Push to CodeCommit

CodeCommit uses **AWS credentials**, so in CloudShell it will automatically use your IAM credentials.

```bash
git push -u origin main
```

* If it asks for credentials, CloudShell already has the IAM user credentials configured.
* After this, your files (`Dockerfile`, `app.py`, `buildspec.yml`) are in CodeCommit.

---

# ✅ Step 6: Connect CodeBuild to CodeCommit

1. Go to **AWS Console → CodeBuild → Build projects → Create project**
2. **Project name:** `codebuild-demo`
3. **Source:** AWS CodeCommit → choose your repository `codebuild-demo`
4. **Environment:**

   * Managed image: Amazon Linux 2
   * Privileged: ✅ Enabled (for Docker)
5. **Buildspec:** Use `buildspec.yml` in the repository (default)
6. **Service role:** Auto-create (ensure it has `AmazonEC2ContainerRegistryPowerUser`)

---

# ✅ Step 7: Start the build

* Click **Start build** → logs will show:

  * `pre_build`: ECR login
  * `build`: Docker build & tag
  * `post_build`: Docker push

* Check **ECR repository** → image should appear.

---

# ✅ Advantages of CodeCommit

* No S3 zipping required
* Works well for CI/CD pipelines
* Integrates with CodeBuild/CodePipeline easily
* Fully managed by AWS → no rate limit issues like Docker Hub

---

Cheitra, if you want, I can now give a **ready-to-use full project folder**:

```
codebuild-demo/
├── app.py
├── Dockerfile (using public ECR Python)
├── buildspec.yml
```

So you can **push to CodeCommit → build in CodeBuild → push to ECR** with **one click**, avoiding all Docker Hub rate limit issues.

Do you want me to do that?

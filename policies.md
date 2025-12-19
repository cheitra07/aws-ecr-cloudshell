You should run this command in **AWS CloudShell** or **any terminal where AWS CLI is configured** with an IAM user that has **permission to attach IAM policies**.

---

### Step-by-step

1. **Open CloudShell**

   * AWS Console → top-right → **CloudShell icon**

2. **Find your CodeBuild service role name**

   * Go to **AWS Console → CodeBuild → codebuild-demo → Environment → Service role**
   * Copy the **role name** (it looks like `codebuild-demo-service-role-xxxxxx`)

---

3. **Run the command in CloudShell**

Replace `<codebuild-service-role-name>` with your actual role name:

```bash
aws iam attach-role-policy \
  --role-name codebuild-demo-service-role-xxxxxx \
  --policy-arn arn:aws:iam::aws:policy/AmazonEC2ContainerRegistryPowerUser
```

✅ If successful, there will be **no output**.

---

4. **Verify the policy is attached**

```bash
aws iam list-attached-role-policies --role-name codebuild-demo-service-role-xxxxxx
```

You should see:

```json
{
    "AttachedPolicies": [
        {
            "PolicyName": "AmazonEC2ContainerRegistryPowerUser",
            "PolicyArn": "arn:aws:iam::aws:policy/AmazonEC2ContainerRegistryPowerUser"
        }
    ]
}
```

---

After this, your CodeBuild project will have **permissions to login to ECR**, and the **pre_build phase should succeed**.

---

If you want, I can give a **full fixed buildspec.yml + exact CloudShell commands** so you can **rebuild CodeBuild from S3 source with ECR login working on first try**.

Do you want me to do that?

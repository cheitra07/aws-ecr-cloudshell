
---

## 1️⃣ Open CloudShell

AWS Console → top-right → **CloudShell icon** → wait for terminal.

---

## 2️⃣ Login to ECR

```bash
aws ecr get-login-password --region us-east-1 \
| docker login --username AWS --password-stdin xxx.amazonaws.com
```

✅ You should see: `Login Succeeded`

---

## 3️⃣ Create a demo folder & app

```bash
mkdir demo1 && cd demo1
```

Create `app.py`:

```bash
cat <<EOF > app.py
print("Hello from ECR CloudShell demo")
EOF
```

---

## 4️⃣ Create Dockerfile

```bash
cat <<EOF > Dockerfile
FROM python:3.10-slim
WORKDIR /app
COPY app.py .
CMD ["python", "app.py"]
EOF
```

---

## 5️⃣ Build Docker image

```bash
docker build -t demo1 .
```

Check images:

```bash
docker images
```

You should see `demo1` listed.

---

## 6️⃣ Tag image for ECR

```bash
docker tag demo1:latest xxx.amazonaws.com/neelu/demo1:v1
```

---

## 7️⃣ Push image to ECR 🚀

```bash
docker push xxx.amazonaws.com/neelu/demo1:v1
```

✅ Go to **AWS Console → ECR → neelu/demo1 → Images**
You should see `v1` listed.

---

## 8️⃣ Test image in CloudShell (optional)

```bash
docker run --rm xxx.amazonaws.com/neelu/demo1:v1
```

Expected output:

```
Hello from ECR CloudShell demo
```

---

aws ecr delete-repository --repository-name neelu/demo1 --region us-east-1 --force
rm -rf ~/demo1
Step 3: Check IAM (optional)

If you created any special IAM user/role → delete

If you used your own IAM → no action needed

Step 4: CloudWatch logs (optional)

CloudShell + Docker commands don’t produce persistent logs

CodeBuild logs will be in CloudWatch → delete if needed

---
Do you want me to do that?

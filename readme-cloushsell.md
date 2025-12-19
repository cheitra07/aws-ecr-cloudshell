
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

## 🎯 Done!

You now have:

* Built a Docker image in CloudShell
* Tagged it
* Pushed it to your **ECR repository**
* Tested it locally in CloudShell

---

If you want, next I can show **how to deploy this image to Lambda or ECS Fargate** without touching Docker locally.

Do you want me to do that?

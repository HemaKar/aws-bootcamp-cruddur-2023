# Week 1 — App Containerization

## Why to create containers?
### Easy to move the application, makes it portable without having to worry about configurations to the environment you deploy. The individual running the app can run it as good as locally. 

## Docker file creation in backend. 
---------------------------------
Installed the extension for docker in gitpod and VSCode. 

Steps to Contairize backend. 
1. Run Python 
```cd backend-flask
export FRONTEND_URL="*"
export BACKEND_URL="*"
python3 -m flask run --host=0.0.0.0 --port=4567
cd ..```


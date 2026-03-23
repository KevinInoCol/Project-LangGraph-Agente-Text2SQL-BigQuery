# Este comando solo se instala si tienes python 3.11
pip install -U "langgraph-cli[inmem]"

# Corre el siguiente comando
langgraph dev --allow-blocking


# Crear su Virtual Environment:
LangGraph-Agente-Texto-SQL

# Activa tu Virtual Environment:
conda activate LangGraph-Agente-Texto-SQL



# Comandos para desplegar en "DigitalOcean" y "DockerHub"
- docker login

- docker buildx build --platform linux/amd64 -t kevininofuentecolque/app-langgraph-agent-big-query-v1:latest --push .
#--------------------------------------------------------------------


# Google Cloud Run, Google Cloud Functions, Google Engine
# AWS, Lambda, EC2, 
# AWS -> ECR
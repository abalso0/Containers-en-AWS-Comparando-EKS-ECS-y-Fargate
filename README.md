# Containers en AWS: ECS con EC2 vs ECS con Fargate

Dos demos con CloudFormation para comparar ECS con EC2 (tú gestionas servidores) vs ECS con Fargate (serverless).

## Demos

| Demo | Compute | Archivo | 
|------|---------|---------|
| 1 | EC2 (tú gestionas) | `demo1-ecs-ec2.yaml` | 
| 2 | Fargate (serverless) | `demo2-ecs-fargate.yaml` |

Ambos despliegan NGINX accesible por HTTP usando ECS como orquestador.

## Requisitos

- AWS CLI configurado
- Permisos para crear VPC, ECS, ALB, IAM Roles

## Demo 1 ECS con EC2

```bash
aws cloudformation deploy \
  --template-file demo1-ecs-ec2.yaml \
  --stack-name demo1-ecs-ec2 \
  --capabilities CAPABILITY_IAM \
  --region us-east-1
```

Obtener la URL:
```bash
aws cloudformation describe-stacks --stack-name demo1-ecs-ec2 \
  --query "Stacks[0].Outputs" --region us-east-1
```

### Tú gestionas las instancias EC2, tipo de instancia, Auto Scaling Group, parches, capacidad.

## Demo 2 ECS con Fargate

```bash
aws cloudformation deploy \
  --template-file demo2-ecs-fargate.yaml \
  --stack-name demo2-ecs-fargate \
  --capabilities CAPABILITY_IAM \
  --region us-east-1
```

Obtener la URL:
```bash
aws cloudformation describe-stacks --stack-name demo2-ecs-fargate \
  --query "Stacks[0].Outputs" --region us-east-1
```

### No hay instancias EC2. Solo defines CPU y memoria, AWS gestiona todo lo demás.

## Comparación

| | ECS + EC2 | ECS + Fargate |
|---|---|---|
| **Gestión de servidores** | Tú | AWS |
| **Complejidad** | Media | Baja |
| **Control** | Total | Limitado |
| **Costo** | Pagas EC2 siempre | Pagas por uso |

## Borra los stacks al terminar para evitar costos.

```bash
aws cloudformation delete-stack --stack-name demo2-ecs-fargate --region us-east-1
aws cloudformation delete-stack --stack-name demo1-ecs-ec2 --region us-east-1
```


# Auto-Scaling

## ALB based solution architecture
- Use Route 53 CNAME Weighted record to route traffic across multiple ALBs. Each ALB can connect to an auto-scaling group.
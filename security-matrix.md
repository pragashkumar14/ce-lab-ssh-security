cat > /mnt/c/Users/praga/cloud-engineering/ce-lab-ssh-security/security-matrix.md << 'EOF'
# Security Configuration Matrix

## Instance Details
- Instance ID: i-0a7fbb1d58070c9af
- Instance Type: t3.micro
- Public IP: 15.188.65.58
- Private IP: 172.31.36.27

## Security Group: week2-web-server-sg
- Group ID: sg-0ee57253c6d5940ec
- VPC ID: vpc-034e913d2933ce958

### Inbound Rules
| Protocol | Port | Source | Purpose | Risk Level |
|----------|------|--------|---------|------------|
| TCP | 22 | 81.220.52.58/32 | SSH admin access | Low (restricted IP) |
| TCP | 80 | 0.0.0.0/0 | Public web traffic | Low (expected) |

### Outbound Rules
| Protocol | Port | Destination | Purpose |
|----------|------|-------------|---------|
| All | All | 0.0.0.0/0 | Unrestricted egress |

## Security Assessment
- ✅ SSH restricted to specific IP
- ✅ No unnecessary ports open
- ✅ HTTP enabled for web server purpose
- ⚠️ Consider: Restrict outbound traffic

## Recommendations
1. Add monitoring alerts for SSH attempts
2. Consider using Session Manager instead of SSH
3. Enable VPC Flow Logs
EOF
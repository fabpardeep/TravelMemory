# TravelMemory AWS Deployment

## Project Overview

TravelMemory is a MERN application deployed on Amazon Web Services. The application allows users to create and view travel-memory records.

The deployment uses separate frontend and backend tiers, two Availability Zones, dedicated Application Load Balancers, Nginx, systemd, and MongoDB Atlas.

## Technologies Used

- React
- Node.js
- Express.js
- MongoDB Atlas
- Amazon EC2
- AWS Application Load Balancers
- Nginx
- systemd
- GitHub

## AWS Region

**US East (N. Virginia)**

```text
us-east-1
```

## Deployment Architecture

The deployment contains:

- Two frontend EC2 instances
- Two backend EC2 instances
- Two Availability Zones
- One frontend Application Load Balancer
- One backend Application Load Balancer
- One frontend target group
- One backend target group
- MongoDB Atlas
- Nginx on frontend and backend instances
- Node.js managed by systemd on backend instances
- Custom frontend and backend AMIs

## Application Communication Flow

```text
User Browser
    |
    v
Frontend Application Load Balancer
    |
    v
Frontend Target Group
    |
    +---- travelmemory-frontend-1
    |
    +---- travelmemory-frontend-2
              |
              v
Backend Application Load Balancer
              |
              v
Backend Target Group
              |
              +---- travelmemory-backend-1
              |
              +---- travelmemory-backend-2
                        |
                        v
                  MongoDB Atlas
```

## EC2 Instances

### Frontend Instances

- `travelmemory-frontend-1`
- `travelmemory-frontend-2`

The frontend instances use Nginx to serve the React production build on HTTP port 80.

### Backend Instances

- `travelmemory-backend-1`
- `travelmemory-backend-2`

The backend instances use:

- Nginx on port 80
- Node.js on internal port 3000
- systemd for automatic service startup and restart

## Load Balancing

### Frontend

- Load balancer: `travelmemory-frontend-alb`
- Target group: `travelmemory-frontend-tg`
- Health-check path: `/`
- Target port: `80`

### Backend

- Load balancer: `travelmemory-backend-alb`
- Target group: `travelmemory-backend-tg`
- Health-check path: `/hello`
- Target port: `80`

## Live Application

```text
http://travelmemory-frontend-alb-1979380227.us-east-1.elb.amazonaws.com/
```

> The four EC2 instances must be running and both target groups must contain healthy targets for the application to remain available.

## Backend Health Endpoint

```text
http://YOUR-BACKEND-ALB-DNS/hello
```

Replace `YOUR-BACKEND-ALB-DNS` with the actual DNS name of `travelmemory-backend-alb` before final submission.

## Documentation

The `docs` folder contains:

- Final deployment report in Word format
- Final deployment report in PDF format
- Editable architecture and process diagram
- Exported architecture image
- Screenshot-backed deployment evidence

## Deployment Process

1. Forked the TravelMemory repository into the candidate GitHub account.
2. Created a MongoDB Atlas project, cluster, database user, and network-access rule.
3. Created tier-specific AWS security groups.
4. Launched frontend and backend master EC2 instances.
5. Installed and configured Node.js, React, Nginx, Git, and systemd.
6. Verified the master instances and application functionality.
7. Created reusable frontend and backend AMIs.
8. Launched a second frontend and backend instance in another Availability Zone.
9. Created frontend and backend target groups.
10. Created separate frontend and backend Application Load Balancers.
11. Updated both React builds to use the backend load-balancer DNS name.
12. Completed health, browser, database, and functional testing.
13. Prepared the deployment report, screenshots, and architecture diagrams.

## Validation Results

- Backend health endpoint: Passed
- Backend database route: Passed
- Frontend HTTP test: Passed
- Backend target group: Two healthy targets
- Frontend target group: Two healthy targets
- Frontend-to-backend communication: Passed
- Create-and-read functional test: Passed
- Service restart and automatic startup tests: Passed

## Security Controls

- The AWS root account was not used for routine deployment.
- SSH access was restricted to the administrator's public IP.
- Node.js port 3000 was not exposed publicly.
- EC2 web traffic was restricted to the corresponding load-balancer security groups.
- MongoDB credentials were stored in `backend/.env` on the backend instances.
- Environment files were excluded from Git.
- Private `.pem` key files were excluded from Git.
- No database passwords, private keys, or full connection strings are included in this repository.

## Files Excluded From Git

```text
backend/.env
frontend/.env
frontend/.env.production
*.pem
*.swp
*.swo
*~
node_modules/
```

## Optional Components

Cloudflare DNS and HTTPS were treated as optional and were not implemented. The application was deployed and validated using the AWS Application Load Balancer DNS names.

## Candidate

Pardeep Singh

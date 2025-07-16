# Tilt Demo Project

This repository demonstrates how to use [Tilt](https://tilt.dev/) to manage multi-service Kubernetes applications with automatic rebuilds and deployments.

## What is Tilt?

Tilt is a development environment orchestrator that makes it easy to work with multi-service applications. It provides:

- **Live Updates**: Automatically rebuilds and deploys your services when you make code changes
- **Multi-Service Management**: Manages multiple services in a single development environment
- **Kubernetes Integration**: Seamlessly works with Kubernetes clusters
- **Resource Monitoring**: Provides a web UI to monitor your services and their status
- **Fast Feedback Loop**: Reduces the time between making changes and seeing them in your application

### Key Benefits:

- **Faster Development**: No need to manually rebuild and redeploy containers
- **Consistent Environment**: Ensures all developers work with the same setup
- **Easy Onboarding**: New team members can get started quickly
- **Production-like Testing**: Test your applications in an environment similar to production

## Project Structure

```
TiltDemo/
├── applications/           # Application source code
│   ├── server1/           # First web service
│   │   ├── Dockerfile     # Container definition for server1
│   │   └── index.html     # Simple HTML page for server1
│   └── server2/           # Second web service
│       ├── Dockerfile     # Container definition for server2
│       └── index.html     # Simple HTML page for server2
└── tilt-stack/            # Tilt configuration
    ├── Tiltfile           # Main Tilt configuration file
    └── app_definitions/   # Kubernetes manifests
        ├── webserver1.yaml # K8s config for server1
        └── webserver2.yaml # K8s config for server2
```

## Services Overview

This demo includes two simple web services:

### Server 1 (webserver1)

- **Port**: 8085
- **Technology**: Nginx with Alpine Linux
- **Purpose**: Demonstrates basic web service setup
- **Access**: http://localhost:8085

### Server 2 (webserver2)

- **Port**: 8083
- **Technology**: Nginx with Alpine Linux
- **Purpose**: Demonstrates multi-service architecture
- **Access**: http://localhost:8083

## Prerequisites

Before running this project, ensure you have:

1. **Docker** installed and running
2. **Kubernetes cluster** (local or remote)
   - [Docker Desktop](https://www.docker.com/products/docker-desktop/) with Kubernetes enabled
   - [Minikube](https://minikube.sigs.k8s.io/)
   - [Kind](https://kind.sigs.k8s.io/)
   - Any other Kubernetes cluster
3. **Tilt** installed
   - Download from [tilt.dev](https://docs.tilt.dev/install.html)
   - Or install via package manager:

     ```bash
     # macOS
     brew install tilt-dev/tap/tilt

     # Linux
     curl -fsSL https://raw.githubusercontent.com/tilt-dev/tilt/master/scripts/install.sh | bash
     ```

## Getting Started

1. **Clone the repository**:

   ```bash
   git clone <your-repo-url>
   cd TiltDemo
   ```

2. **Navigate to the tilt-stack directory**:

   ```bash
   cd tilt-stack
   ```

3. **Start Tilt**:

   ```bash
   tilt up
   ```

4. **Access the services**:
   - Server 1: http://localhost:8085
   - Server 2: http://localhost:8083
   - Tilt UI: http://localhost:10350 (default Tilt dashboard)

## How It Works

### Tiltfile Configuration

The `Tiltfile` orchestrates the entire development workflow:

```python
# Build Docker images
docker_build('webserver1','../applications/server1/')
docker_build('webserver2','../applications/server2/')

# Deploy Kubernetes resources
k8s_yaml('app_definitions/webserver1.yaml')
k8s_yaml('app_definitions/webserver2.yaml')

# Configure port forwarding
k8s_resource('webserver1', port_forwards=8085)
k8s_resource('webserver2', port_forwards=8083)
```

### Development Workflow

1. **Initial Setup**: Tilt builds Docker images and deploys to Kubernetes
2. **Live Updates**: When you modify files in `applications/`, Tilt automatically:
   - Rebuilds the affected Docker image
   - Redeploys the Kubernetes resources
   - Updates the service
3. **Monitoring**: The Tilt UI shows real-time status of all services

## Making Changes

To see Tilt in action:

1. **Modify a service**: Edit `applications/server1/index.html` or `applications/server2/index.html`
2. **Watch the magic**: Tilt automatically rebuilds and redeploys the service
3. **Refresh your browser**: See the changes immediately

## Useful Commands

```bash
# Start Tilt
tilt up

# Stop Tilt
tilt down

# View logs
tilt logs

# Trigger a rebuild
tilt trigger webserver1

# Check status
tilt get uiresource
```

## Customization

### Adding a New Service

1. Create a new directory in `applications/`
2. Add your application code and Dockerfile
3. Create a Kubernetes manifest in `tilt-stack/app_definitions/`
4. Add the service to the `Tiltfile`:
   ```python
   docker_build('newservice', '../applications/newservice/')
   k8s_yaml('app_definitions/newservice.yaml')
   k8s_resource('newservice', port_forwards=8080)
   ```

### Using a Custom Docker Registry

Uncomment and modify the registry line in `Tiltfile`:

```python
default_registry('your-docker-registry.local')
```

## Troubleshooting

### Common Issues

1. **Port conflicts**: Ensure ports 8083 and 8085 are available
2. **Kubernetes not running**: Start your Kubernetes cluster
3. **Docker not running**: Start Docker Desktop or Docker daemon
4. **Permission issues**: Ensure you have proper permissions for Docker and Kubernetes

### Getting Help

- Check the [Tilt documentation](https://docs.tilt.dev/)
- Join the [Tilt Slack community](https://kubernetes.slack.com/archives/CESBL84MV)
- Review the [Tilt GitHub repository](https://github.com/tilt-dev/tilt)

## Resources and Links

### Official Tilt Resources

- [Tilt Website](https://tilt.dev/)
- [Tilt Documentation](https://docs.tilt.dev/)
- [Tilt GitHub Repository](https://github.com/tilt-dev/tilt)
- [Installation Guide](https://docs.tilt.dev/install.html)
- [Getting Started Tutorial](https://docs.tilt.dev/tutorial.html)

### Community and Support

- [Tilt Slack Channel](https://kubernetes.slack.com/archives/CESBL84MV)
- [Tilt Blog](https://blog.tilt.dev/)
- [Tilt Examples](https://github.com/tilt-dev/tilt-example-docker)

### Related Tools

- [Kubernetes Documentation](https://kubernetes.io/docs/)
- [Docker Documentation](https://docs.docker.com/)
- [Minikube](https://minikube.sigs.k8s.io/)
- [Kind](https://kind.sigs.k8s.io/)

## Contributing

Feel free to submit issues, feature requests, or pull requests to improve this demo project.

## License

This project is open source and available under the [MIT License](LICENSE).

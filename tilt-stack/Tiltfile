#default_registry('your-docker-registry.local')  # si no usas registry, omite esto

#Server 1
docker_build('webserver1','../applications/server1/')
k8s_yaml('app_definitions/webserver1.yaml')
k8s_resource('webserver1', port_forwards=8085)

#Server 2
docker_build('webserver2','../applications/server2/')
k8s_yaml('app_definitions/webserver2.yaml')
k8s_resource('webserver2', port_forwards=8083)



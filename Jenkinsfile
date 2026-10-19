pipeline {
    agent any

    environment {
        DOTNET_ROOT = "${WORKSPACE}/.dotnet"
        PATH        = "${WORKSPACE}/.dotnet:${env.PATH}"
        DOTNET_SYSTEM_GLOBALIZATION_INVARIANT = '1'

        BIND_MOUNT_PATH = "/mnt/host-rise-server"
        HOST_GATEWAY    = "host.docker.internal"
        HOST_USER       = "vagrant"
        SERVICE_NAME    = "rise-server.service"
    }

    stages {
        stage("Checkout") {
            steps {
                git branch: 'main', url: 'https://github.com/HOGENT-RISE/dotnet-2627-template.git'
            }
        }

        stage("Install .NET SDK") {
            steps {
                sh '''
                    if [ ! -f "$DOTNET_ROOT/dotnet" ]; then
                        curl -sSL https://dot.net/v1/dotnet-install.sh | bash /dev/stdin --channel 10.0 --install-dir "$DOTNET_ROOT"
                    fi
                '''
            }
        }

        stage("Build & Publish to Mount") {
            steps {
                sh '''
                    mkdir -p "${BIND_MOUNT_PATH}"
                    dotnet publish src/Rise.Server -c Release -o "${BIND_MOUNT_PATH}"
                '''
            }
        }
    }
}
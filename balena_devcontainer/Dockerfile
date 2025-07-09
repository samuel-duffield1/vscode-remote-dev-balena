FROM ubuntu:22.04

# Install basic dependencies, and SSH server
RUN apt-get update && apt-get install -y \
    build-essential \
    pkg-config \
    libssl-dev \
    ca-certificates \
    openssh-server \
    supervisor 

# Configure SSH
RUN mkdir /var/run/sshd
RUN echo 'root:password12345' | chpasswd
RUN sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN sed -i 's/#PasswordAuthentication yes/PasswordAuthentication yes/' /etc/ssh/sshd_config
RUN sed -i 's/#Port 22/Port 7878/' /etc/ssh/sshd_config
# Generate SSH host keys
RUN ssh-keygen -A

# Create supervisor config for running SSH in the background
RUN mkdir -p /etc/supervisor/conf.d
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# Expose SSH port
EXPOSE 7878

# Use supervisor to run both SSH and the application
CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]
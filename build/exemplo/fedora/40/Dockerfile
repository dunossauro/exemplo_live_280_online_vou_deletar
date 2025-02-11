FROM fedora:40

# Set the working directory
WORKDIR /app

# Disable pip's warnings and SDL audio
ENV PIP_ROOT_USER_ACTION=ignore \
    PIP_NO_WARN_SCRIPT_LOCATION=0 \
    SDL_AUDIODRIVER=dummy

# Install System python
RUN dnf install -y \
      python3-devel \
      python3-pip

# Upgrade pip et alia
# PEP 668 allows distros to mark the system Python as "externally managed". With this
# configuration in the distro, `pip install` will error when using the system Python.
# PIP_BREAK_SYSTEM_PACKAGES allows pip to ignore this safeguard and when combined with
# PIP_IGNORE_INSTALLED, pip will altogether ignore the distro-managed versions of these
# packages and install the latest versions to /usr/local/.
RUN PIP_BREAK_SYSTEM_PACKAGES=1 \
    PIP_IGNORE_INSTALLED=1 \
    python3 -m pip install --no-cache-dir --upgrade pip setuptools wheel

# As root, install system packages required by app
ARG SYSTEM_REQUIRES
RUN dnf install -y gcc make rpm-build pkgconf-pkg-config git ${SYSTEM_REQUIRES}

# Ensure Docker user UID:GID matches host user UID:GID (beeware/briefcase#403)
# Use --non-unique to avoid problems when the UID:GID of the host user
# collides with entries provided by the Docker container.
ARG HOST_UID
ARG HOST_GID
RUN groupadd --non-unique --gid $HOST_GID briefcase && \
    useradd --non-unique --uid $HOST_UID --gid $HOST_GID brutus --home /home/brutus && \
    mkdir -p /home/brutus && chown brutus:briefcase /home/brutus

# Use the brutus user for operations in the container
USER brutus

# ========== START USER PROVIDED CONTENT ==========


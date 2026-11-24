# Scaling to a team

This is one server with no failover. A team deployment would add:

- **Single sign-on and a directory,** so people log in once and permissions follow their team.
- **Per-team document access,** so each person retrieves only what they are allowed to see.
- **Backups** of host configuration and service data, with tested restores.
- **Monitoring and alerts** for GPUs, disks, and service health.
- **A higher-throughput inference server** with batching for many simultaneous users.
- **Redundancy,** or at minimum a fast rebuild from stored configuration.

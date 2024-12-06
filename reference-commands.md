# Docker Filesystem Changes Command Reference Guide

### Project content table
- [Section 1: Core Project Workflow](#section-1-core-project-workflow)
- [Section 2: Advanced Operations](#section-2-advanced-operations)
- [Section 3: Production Guide](#section-3-production-guide)

> **Author**: [Md Toriqul Islam](https://linkedin.com/in/thetoriqul)  
> **Description**: Comprehensive guide for Docker filesystem operations and change tracking  
> **Learning Focus**: Understanding Docker's Union Filesystem and container layer management  
> **Note**: Review commands carefully before execution in production environments

## Section 1: Core Project Workflow

### Step 1: Creating and Adding Files
```bash
# Create a container and add a new file
docker container run --name tweak-a busybox:latest touch /HelloWorld

# Verification command
docker container diff tweak-a
```

### Step 2: Deleting Files
```bash
# Create a container and delete an existing file
docker container run --name tweak-d busybox:latest rm /bin/vi

# Verification command
docker container diff tweak-d
```

### Step 3: Modifying Files
```bash
# Create a container and modify an existing file
docker container run --name tweak-c busybox:latest touch /bin/vi

# Verification command
docker container diff tweak-c
```

### Final Step: Cleanup
```bash
# Remove all test containers
docker container rm -vf tweak-a
docker container rm -vf tweak-d
docker container rm -vf tweak-c
```

## Section 2: Advanced Operations

### Advanced Container Inspection
```bash
# View detailed container information
docker container inspect <container-name>

# Export container filesystem
docker container export <container-name> > container-fs.tar

# Analyze specific file changes
docker container exec <container-name> ls -l /path/to/file
```

### Layer Management
```bash
# View layer history
docker history <image-name>

# Commit container changes
docker container commit <container-name> <new-image-name>

# Compare container changes
docker container diff <container-name>
```

## Section 3: Production Guide

### Production Setup
```bash
# Create production-ready container
docker container run -d \
    --name prod-container \
    --restart unless-stopped \
    busybox:latest

# Monitor filesystem changes
docker container diff prod-container
```

### Monitoring
```bash
# Track real-time changes
docker container stats prod-container

# View logs
docker container logs -f prod-container
```

### Backup Operations
```bash
# Export container filesystem
docker container export prod-container > backup.tar

# Create container snapshot
docker container commit prod-container backup-image
```

## Learning Notes

1. Changes are tracked at the container layer
2. Copy-on-write occurs when modifying files
3. Directory changes are marked with 'C'
4. File deletions persist across container restarts
5. Changes are relative to the base image

---

> 💡 **Best Practice**: Always verify filesystem changes before committing them to new images

> ⚠️ **Warning**: File deletions in base layers require rebuilding the image

> 📝 **Note**: The `diff` command shows changes since container creation
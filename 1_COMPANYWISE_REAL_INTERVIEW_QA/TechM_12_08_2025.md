Q) command for to give  permission for file?
   chmod [permissions] filename
   
Q) command to change the ownership ?
   To change the ownership of a file in Linux/Unix, you use the chown command.
   chown [new_owner] filename



Q) write a shell script to download artifact , update binary in /webapp?
   Download an artifact from a URL.
   Replace the binary in /webapp with the downloaded one.
   Keep a backup of the old binary just in case.
#!/bin/bash
# Script to download artifact and update binary in /webapp

# ===== CONFIGURATION =====
ARTIFACT_URL="https://example.com/path/to/artifact.tar.gz"
WEBAPP_DIR="/webapp"
BINARY_NAME="myapp"    # Name of the binary inside /webapp
TMP_DIR="/tmp/webapp_update"

# ===== SCRIPT START =====
set -e  # Exit immediately if a command fails

echo "[INFO] Creating temporary directory..."
mkdir -p "$TMP_DIR"

echo "[INFO] Downloading artifact from $ARTIFACT_URL..."
curl -L -o "$TMP_DIR/artifact.tar.gz" "$ARTIFACT_URL"

echo "[INFO] Extracting artifact..."
tar -xzf "$TMP_DIR/artifact.tar.gz" -C "$TMP_DIR"

# Assuming the binary is in the extracted folder
NEW_BINARY_PATH=$(find "$TMP_DIR" -type f -name "$BINARY_NAME" | head -n 1)

if [ -z "$NEW_BINARY_PATH" ]; then
    echo "[ERROR] Binary $BINARY_NAME not found in artifact."
    exit 1
fi

echo "[INFO] Backing up existing binary..."
if [ -f "$WEBAPP_DIR/$BINARY_NAME" ]; then
    cp "$WEBAPP_DIR/$BINARY_NAME" "$WEBAPP_DIR/${BINARY_NAME}.bak_$(date +%F_%T)"
fi

echo "[INFO] Updating binary in $WEBAPP_DIR..."
cp "$NEW_BINARY_PATH" "$WEBAPP_DIR/$BINARY_NAME"
chmod +x "$WEBAPP_DIR/$BINARY_NAME"

echo "[INFO] Cleaning up..."
rm -rf "$TMP_DIR"

echo "[SUCCESS] Binary updated successfully."





Q) To extract a  tar file specify the command ?
   tar -xf file.tar



Q) how to check kubernetes pods?
   kubectl get pods


Q)In kubernetes how to check logs ?
 kubectl logs <pod-name>

Q)command to install  any service in linux ?
  For RHEL/CentOS/Fedora ---- 
  # Older systems:
  sudo yum install <package-name>
  # Newer systems:
  sudo dnf install <package-name>


Q)write a basic docker file ?

# Use an official base image
FROM python:3.10-slim

# Set working directory inside the container
WORKDIR /app

# Copy application files into the container
COPY . /app

# Install dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Expose the application port
EXPOSE 5000

# Command to run the app
CMD ["python", "app.py"]


Q)In linux how to check application ports which are running ?
  
   sudo ss -tulnp

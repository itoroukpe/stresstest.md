To induce load on a Linux server, you can use stress-ng, a tool specifically designed for stress testing and benchmarking systems. Below is an example of a basic stress test script using stress-ng:

```bash
#!/bin/bash

# Install stress-ng if not already installed
sudo apt-get update
sudo apt-get install stress-ng -y

# Define variables
CPU_CORES=$(nproc --all)  # Get number of CPU cores
MEMORY_SIZE=$(free -m | awk '/Mem:/ {print $2}')  # Get total memory size in MB

# Define stress-ng parameters
CPU_LOAD_PERCENTAGE=50  # Adjust the CPU load percentage as needed
MEMORY_LOAD_PERCENTAGE=50  # Adjust the memory load percentage as needed

# Run stress-ng tests
echo "Starting stress-ng tests..."

# CPU stress test
echo "Running CPU stress test with $CPU_LOAD_PERCENTAGE% load on $CPU_CORES CPU cores..."
stress-ng --cpu $CPU_CORES --cpu-load $CPU_LOAD_PERCENTAGE &

# Memory stress test
echo "Running memory stress test with $MEMORY_LOAD_PERCENTAGE% load on $MEMORY_SIZE MB memory..."
stress-ng --vm 1 --vm-bytes $MEMORY_SIZE"M" --vm-keep --vm-hang $MEMORY_LOAD_PERCENTAGE% &

# Wait for stress tests to complete
echo "Waiting for stress tests to complete..."
wait

echo "Stress tests completed."
```

Save the above script to a file, such as `stress_test.sh`, and make it executable using the following command:

```bash
chmod +x stress_test.sh
```

Then, you can run the script to start the stress test:

```bash
./stress_test.sh
```

This script will run stress-ng tests to induce load on the CPU and memory of the server. You can adjust the `CPU_LOAD_PERCENTAGE` and `MEMORY_LOAD_PERCENTAGE` variables to control the intensity of the load. Additionally, you may want to monitor system performance during the stress test to ensure that it does not cause excessive resource usage or impact other applications running on the server.

To conduct a stress test on a Linux-based system to induce load and test horizontal autoscaling in AWS, you can use a tool called `stress`. `stress` is a simple command-line utility that imposes load on a system by stressing CPU, memory, I/O, or other resources. Here's a basic script using `stress` to induce load on a Linux system:

```bash
#!/bin/bash

# Install stress if not already installed
sudo apt-get update
sudo apt-get install -y stress

# Stress CPU with 4 workers for 60 seconds
echo "Stressing CPU..."
stress --cpu 4 --timeout 60s

# Stress memory with 1GB of memory for 60 seconds
echo "Stressing Memory..."
stress --vm 1 --vm-bytes 1G --timeout 60s

# Stress I/O with 4 workers for 60 seconds
echo "Stressing I/O..."
stress --io 4 --timeout 60s

# Stress CPU and memory simultaneously with 2 workers each for 60 seconds
echo "Stressing CPU and Memory..."
stress --cpu 2 --io 2 --vm 2 --timeout 60s

# Remove stress after test completion (optional)
sudo apt-get remove -y stress
```

Save this script as `stress_test.sh` and make it executable using the following command:

```bash
chmod +x stress_test.sh
```

Then, you can run the script to induce load on your system:

```bash
./stress_test.sh
```

This script will stress the CPU, memory, and I/O of the system with different configurations for 60 seconds each. You can adjust the parameters (e.g., number of CPU workers, memory size) based on your specific testing requirements.

To use this script to test horizontal autoscaling in AWS:

1. Launch an auto scaling group with multiple instances.
2. Configure auto scaling policies to scale out (add instances) based on CPU or other metrics.
3. Run the stress test script on an instance within the auto scaling group to induce load.
4. Monitor the auto scaling group to observe how it responds to the increased load by adding additional instances.
5. Once the load decreases, observe how the auto scaling group scales in (removes instances) to maintain the desired capacity.

This approach allows you to validate the effectiveness of horizontal autoscaling in AWS under different levels of load and ensure that your application can dynamically adjust its capacity to handle fluctuations in demand.


## To induce load on a Linux server, you can use stress-ng, a tool specifically designed for stress testing and benchmarking systems. Below is an example of a basic stress test script using stress-ng:

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

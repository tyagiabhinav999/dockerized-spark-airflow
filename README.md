# Local Spark Cluster with Docker Compose (Soon with Airflow!)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT) <!-- Choose your license -->

This repository provides a straightforward Docker Compose setup to easily run an Apache Spark cluster (Master + Worker(s)) locally for development and testing. It simplifies the process of getting a Spark environment running without needing manual installations on your host machine.

** Roadmap:** The next major step for this project is to integrate a Dockerized Apache Airflow instance, creating a convenient, unified local environment for developing and testing data pipelines involving both Spark and Airflow.

## Features ✨

*   **Easy Setup:** Launch a multi-node Spark cluster with a single command.
*   **Isolated Environment:** Keeps Spark dependencies contained within Docker.
*   **Cross-Platform:** Works on macOS, Windows (with WSL2/Docker Desktop), and Linux.
*   **Configurable:** Easily adjust Spark versions (by modifying the Dockerfile or image tag) and scale workers.
*   **Foundation for Airflow:** Designed to be extended with an Airflow service soon.

## Prerequisites 🔧

Before you begin, ensure you have the following installed:

*   [Docker Engine](https://docs.docker.com/engine/install/)
*   [Docker Compose](https://docs.docker.com/compose/install/) (v2 plugin recommended - i.e., you use `docker compose` not `docker-compose`)
*   [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) (for cloning the repository)

## Getting Started 🚀

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/[your-github-username]/[repository-name].git
    cd [repository-name]
    ```

2.  **Build Context Files (If needed):**
    *   This setup requires a `log4j2.properties` file for Spark logging configuration during the image build process (as defined in `Dockerfile.spark`).
    *   A default `log4j2.properties` should be included in this repository's root directory. If you need to customize logging, modify this file before building.

3.  **Build and Run the Services:**
    *   Use the `docker compose up` command. The `-d` flag runs containers in the background (detached mode). The `--build` flag ensures the Spark images are built using the included `Dockerfile.spark` and `log4j2.properties`.
    ```bash
    docker compose up --build -d
    ```

4.  **Verify the Services:**
    *   Check if the containers are running:
    ```bash
    docker compose ps
    ```
    *   You should see services like `spark-master` and `spark-worker` with a status of `running` or `up`.

## Accessing Services 🌐

*   **Spark Master UI:** Open your web browser and navigate to [http://localhost:8080](http://localhost:8080) (or the host port you mapped for the Spark Master's 8080 port in `docker-compose.yml`). You should see the Spark Master dashboard where you can monitor applications and workers.
*   **Submitting Spark Jobs:** You can use `docker compose exec` to run `spark-submit` commands directly within the `spark-master` container. (Further examples might be added later).

## Configuration ⚙️

*   **Scaling Workers:** To run more Spark worker nodes, use the `--scale` option:
    ```bash
    # Example: Run 3 worker nodes
    docker compose up -d --scale spark-worker=3
    ```
*   **Ports:** Service ports are defined in the `docker-compose.yml` file. Adjust the `host:container` mappings if needed (e.g., if `8080` is already in use on your host).
*   **Spark Version:** The Spark version is determined by the base image specified in the `FROM` instruction within `Dockerfile.spark`. Change the tag (e.g., `bitnami/spark:3.4.0`) and rebuild (`docker compose up --build -d`) to use a different version.
*   **Logging:** Modify the `log4j2.properties` file to change Spark's logging verbosity and configuration. Rebuild the images after changes.
*   **Resources:** You can add resource constraints (memory, CPU) to services within the `docker-compose.yml` file using the `deploy` key (refer to Docker Compose documentation).

## Stopping the Cluster 🛑

*   To stop and remove the containers, networks, and default volumes created by Compose:
    ```bash
    docker compose down
    ```
*   If you want to remove named volumes as well ( **Caution: This deletes Spark data if persisted in named volumes!**):
    ```bash
    docker compose down -v
    ```

## Roadmap / Future Work 🛣️

*   [ ] **Integrate Apache Airflow:** Add Airflow services (webserver, scheduler, worker, etc.) to the `docker-compose.yml`.
*   [ ] Provide example DAGs demonstrating Spark job submission from Airflow.
*   [ ] Add options for persistent storage for Spark/Airflow metadata.
*   [ ] Include basic examples of `spark-submit` usage.

## Contributing 🤝

Contributions are welcome! Please feel free to submit pull requests or open issues to improve this setup.

1.  Fork the Project
2.  Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3.  Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4.  Push to the Branch (`git push origin feature/AmazingFeature`)
5.  Open a Pull Request

## License 📜

Distributed under the MIT License. See `LICENSE` file for more information.

<!-- Add this line if you create a LICENSE file -->
<!-- [LICENSE](LICENSE) -->

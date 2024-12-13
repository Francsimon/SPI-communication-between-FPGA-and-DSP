# SPI communication between FPGA and DSP

Tis project implements the serial peripheral interface (SPI) protcol between an Altera FPGA and a Texas Instruments DSP.
The main feature of this project is the sampling and tranmission schmeme of the data, which was designed to minimize the delays between the sapling of the data from the ADC
in the DSP and the SPI transission of the data from DSP to FPGA. Refer to the published paper for further details.

---

## Table of Contents

1. [Overview](#overview)
2. [Platform Targets](#platform-targets)
3. [Installation](#installation)
4. [Usage Instructions](#usage-instructions)
5. [Contributing](#contributing)
6. [License](#license)

---

## Overview

The FPGA implements the Nios II soft processor to drive the SPI communication of six SPI master modules (this number can be easily extended).
Via C programming, is it possible to code Nios II to transmit data via one SPI module, read the received data, start the SPI communication and monitor if the transmission is finished.
By using the SPI modules provided by Altera, it is only possible to start the SPI communication via software. Hence, if we have multiple SPI modules, we have to execute one
line of code for each of them, starting the transmissions sequentially.
It creates a small delay between subsequent communications, i.e., about 600 ns. If we consider six SPI communications, like in this project, there is a 300 us delay between
the first and last transmission. This phenomenon can be undesirable for hard-time-constrained applications.

This project overcomes this drawback by using an ad-hoc designed SPI module.
The major feature of this module is the presence of a start operation signal that allows the communication to start via HW.
The start operation signal of each SPI module is connected to a register, accessible from Nios II.
By writing into this register, Nios II can start communications with multiple SPI peripherals with a single line of code, establishing the SPI transmissions concurrently!

Moreover, the implementation of the sample & transmission of the data was designed to minimize the delay on the DSP.
The start of conversion of the ADC was connected to the SPI-CS, such as to start acquiring the data soon after the master FPGA asks for them. 
An interrupt is triggered after the conversion and the ISR stores the value into the register of the SPI slave module.
The SPI master module on the FPGA was designed to wait for a delay between the SPI-CS and SPI-CLK to allow the correct sample & store from the DSP.

This sample & communication scheme resulted in minimizing the delays in the ADC sampling & SPI transmission of the data from DSP to FPGA.

For example:

> Project Name is a cross-platform tool designed to streamline task management and increase productivity. It supports task automation, team collaboration, and integrates with popular third-party services.

---

## Platform Targets

This project is compatible with the following platforms:

- **Desktop**:
  - Windows (10 and later)
  - macOS (11 Big Sur and later)
  - Linux (Ubuntu 20.04 and later, Fedora 34+)

- **Mobile**:
  - iOS (14 and later)
  - Android (10 and later)

- **Web**:
  - Latest versions of Chrome, Firefox, Safari, and Edge.

---

## Installation

### Prerequisites

Before installing, ensure you have the following installed:

- [Node.js](https://nodejs.org/) (v16.0.0 or later)
- [Git](https://git-scm.com/)
- A code editor like [Visual Studio Code](https://code.visualstudio.com/).

### Steps

1. Clone the repository:

   ```bash
   git clone https://github.com/your-username/your-repo.git
   cd your-repo
   ```

2. Install dependencies:

   ```bash
   npm install
   ```

3. Build the project (if applicable):

   ```bash
   npm run build
   ```

4. Run the project:

   ```bash
   npm start
   ```

---

## Usage Instructions

### Running Locally

After following the installation steps, you can run the project locally using:

```bash
npm start
```

The application will be available at [http://localhost:3000](http://localhost:3000).

### Deployment

For deploying to a production environment:

1. Build the production version:
   ```bash
   npm run build
   ```

2. Deploy the contents of the `dist` or `build` folder to your server or hosting provider.

---

## Contributing

We welcome contributions! To contribute:

1. Fork the repository.
2. Create a new branch (`git checkout -b feature-name`).
3. Make your changes and commit them (`git commit -m 'Add feature'`).
4. Push to your branch (`git push origin feature-name`).
5. Open a Pull Request.

Please follow our [Code of Conduct](CODE_OF_CONDUCT.md) and [Contributing Guidelines](CONTRIBUTING.md).

---

## License

This project is licensed under the [MIT License](LICENSE). See the LICENSE file for details.
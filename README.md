# CS1237 24-Bit High-Precision ADC Module

This repository contains information and resources for the CS1237, a high-precision, low-power Analog-to-Digital Converter (ADC) module with a single differential input channel.

## Schematic Diagram
![Schematic Diagram](https://github.com/yasir-shahzad/CS1237-24-Bit-ADC-Module/blob/master/Images/Schematic.png)

## PCB Files
![PCB Board](https://github.com/yasir-shahzad/CS1237-24-Bit-ADC-Module/blob/master/Images/PCB%20Board.png)

## Features

- **24-Bit Resolution**: Ensures no missing codes, providing high accuracy and precision.
- **Programmable Gain Amplifier (PGA)**: Supports gains of 1, 2, 64, or 128 (default) to accommodate different signal levels.
- **Selectable Output Data Rates**: Choose from 10Hz, 40Hz, 640Hz, or 1.28kHz to match your application's needs.
- **Stable Reference Voltage**: Onboard TL431 voltage regulator provides a stable 2.5V reference voltage.
- **Built-in Temperature Sensor**: Allows for temperature compensation in measurements.
- **Low Power Consumption**: Ideal for battery-powered and portable applications.
- **2-Wire SPI Interface**: Enables communication with microcontrollers at speeds up to 1.1MHz.

## Applications

- **Industrial Process Control**: Suitable for precise measurements in industrial environments.
- **Electronic Balances**: Provides high accuracy for weight measurement systems.
- **Liquid/Gas Analysis**: Useful in analytical instruments requiring precise digital conversion.
- **Hematology Equipment**: Applicable in medical devices for blood analysis.
- **Intelligent Converters**: Can be integrated into smart conversion devices.
- **Portable Measurement Devices**: Ideal for portable and handheld measuring instruments.

## ⚠️ Important: AVDD / Reference Current Budget for 350Ω Load Cells

The onboard TL431 voltage reference is current-limited by resistor **R1 (1kΩ)**. While sufficient for high-impedance load cells ($\ge 1.7\text{k}\Omega$), it cannot supply enough current for standard **350Ω full-bridge load cells**.

### Issue Summary
At 2.5V excitation, a 350Ω bridge draws **~7.1mA** ($2.5\text{V} / 350\\Omega$). Adding the TL431 minimum bias current (~1mA), the total required budget is **~8.1mA**. 

The stock resistor **R1 = 1kΩ** severely restricts current:
* **At DVDD = 3.3V:** $I = (3.3\text{V} - 2.5\text{V}) / 1\text{k}\Omega = \mathbf{0.8\text{mA}}$ *(insufficient)*
* **At DVDD = 5.0V:** $I = (5.0\text{V} - 2.5\text{V}) / 1\text{k}\Omega = \mathbf{2.5\text{mA}}$ *(insufficient)*

**Symptom:** The reference voltage collapses well below 2.5V under load, dropping below the CS1237 minimum reference threshold (1.5V) and causing saturated or stuck ADC readings.

### Fix
Lower the effective series resistance by adding a resistor between the **DVDD** and **AVDD** nodes or soldering a resistor directly in parallel with **R1**:

| DVDD Voltage | Parallel Resistor | Equivalent R1 | Available Current | Notes |
| :--- | :--- | :--- | :--- | :--- |
| **3.3V / 5.0V** | **100Ω** | ~91Ω | ~8.8mA at 3.3V<br>~27.4mA at 5.0V | **Universal:** Works reliably for both 3.3V and 5V rails. |
| **5.0V Only** | **220Ω** | ~180Ω | ~13.9mA at 5.0V | **5V Only:** Lower quiescent power, but insufficient for 3.3V. |

> **Note:** Measure the resistance across the excitation terminals (`+E` / `-E`) with a multimeter if you are unsure of your load cell's bridge impedance.

## Resources

- **Datasheet**: [CS1237 Datasheet](https://github.com/yasir-shahzad/CS1237-24-Bit-ADC-Module/blob/master/documents/cs1237_datasheet.pdf)

## Getting Started

1. **Download the Repository**: Clone or download the repository to your local machine.
2. **Refer to the Datasheet**: Consult the datasheet for detailed specifications, electrical characteristics, and register descriptions.
3. **SPI Interface Configuration**: Use the SPI interface to configure the CS1237 for your specific application, including channel selection, PGA gain, and output rate.
4. **Read Digital Data**: Retrieve the converted digital data from the CS1237 using your microcontroller or processing unit.

## Community

For questions, discussions, or contributions regarding the CS1237, please feel free to create issues or pull requests in this repository. Your participation is highly valued!

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

Special thanks to the developers and contributors who made this project possible.

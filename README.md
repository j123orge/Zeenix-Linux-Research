# Zeenix-Linux-Research
Research and documentation project focused on Zeenix Lite hardware, OEM Windows drivers, Linux/Bazzite optimization, ACPI, sensors, gyro support and handheld integration.

# Zeenix Linux Research

Projeto comunitário de pesquisa, documentação e otimização do Zeenix Lite no Linux/Bazzite.

Objetivos:

* Mapear hardware real do Zeenix Lite
* Comparar drivers OEM Windows vs Linux/Bazzite
* Identificar módulos corretos do kernel Linux
* Melhorar suporte handheld
* Investigar gyro/sensores/ACPI/I2C
* Criar um perfil otimizado para Bazzite/Zeenix
* Preservar documentação técnica do hardware

---

# Estrutura recomendada do repositório

```text
Zeenix-Linux-Research/
│
├── README.md
├── LICENSE
├── .gitignore
│
├── docs/
│   ├── discoveries/
│   ├── timelines/
│   └── notes/
│
├── hardware/
│   ├── motherboard/
│   ├── cooling/
│   ├── controllers/
│   ├── sensors/
│   └── display/
│
├── windows-drivers/
│   ├── amd/
│   ├── realtek/
│   ├── intel/
│   ├── bosch/
│   └── unknown/
│
├── linux-bazzite/
│   ├── modules/
│   ├── configs/
│   ├── services/
│   ├── scripts/
│   └── logs/
│
├── acpi/
│   ├── dsdt/
│   ├── tables/
│   └── dumps/
│
├── gyro/
│   ├── bmi160/
│   ├── tests/
│   └── linux/
│
├── control-center/
│   ├── dll-analysis/
│   ├── services/
│   ├── ghidra/
│   └── reverse-engineering/
│
├── benchmarks/
│   ├── linux/
│   ├── windows/
│   └── comparisons/
│
└── screenshots/
```

---

# Primeiras descobertas

## BMI160 Gyroscope

Status: CONFIRMADO

Driver Windows:

* BMI160.inf
* BMI160.dll

Hardware IDs encontrados:

```text
ACPI\BOSC0260
ACPI\BMI0260
ACPI\BOSC0160
ACPI\BMI0160
```

Stack identificada:

```text
BMI160 Sensor
↓
AMD I2C
↓
ACPI
↓
UMDF
↓
SensorsCx
↓
Windows Sensor API
```

Hipótese Linux:

* driver bmi160
* subsystem IIO
* integração via ACPI/I2C

---

# Drivers encontrados no pacote OEM

## AMD

* GPU Radeon
* Vulkan
* OpenGL
* ACP
* GPIO
* I2C
* PSP
* SMBUS
* Crash Defender

## Bosch

* BMI160 gyro/accelerometer

## Intel

* Wi-Fi
* Bluetooth

## Realtek

* Áudio
* Leitor SD

---

# Metodologia do projeto

1. Extrair drivers OEM Windows
2. Identificar hardware IDs
3. Mapear stack utilizada no Windows
4. Comparar com Bazzite/Linux
5. Descobrir módulos Linux equivalentes
6. Testar funcionamento
7. Criar documentação
8. Criar perfil otimizado Zeenix

---

# Comandos Linux úteis

## PCI

```bash
lspci -nnk
```

## USB

```bash
lsusb -tv
```

## Módulos carregados

```bash
lsmod
```

## Sensores/IIO

```bash
find /sys/bus/iio/devices -maxdepth 3 -type f
```

## ACPI

```bash
find /sys/bus/acpi/devices -maxdepth 2 -type f -name hid
```

## Logs relevantes

```bash
dmesg | grep -Ei "amd|i2c|gpio|bmi|bosch|sensor|gyro|acpi|hid"
```

---

# Auditoria de drivers

| Hardware    | Windows OEM | Linux/Bazzite | Status     |
| ----------- | ----------- | ------------- | ---------- |
| BMI160 Gyro | BMI160.dll  | investigar    | em análise |
| Radeon Vega | AMD oficial | amdgpu        | OK         |
| AMD I2C     | amdi2c      | investigar    | pendente   |
| AMD GPIO    | amdgpio2    | investigar    | pendente   |
| Realtek SD  | rtsper      | investigar    | pendente   |
| Intel Wi-Fi | netwtw      | iwlwifi       | OK         |

---

# Objetivo futuro

Criar um perfil otimizado:

```text
Bazzite Zeenix Edition
```

Com:

* gyro funcional
* rotação correta
* handheld integration
* power management otimizado
* melhor performance
* menos serviços desnecessários
* configuração específica para Zeenix Lite

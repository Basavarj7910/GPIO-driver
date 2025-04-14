
# GPIO Driver for STM32F407VG

This is a lightweight and flexible GPIO driver written from scratch for the STM32F407VG microcontroller. It includes APIs to configure GPIO pins, manage output/input, and handle interrupt and alternate function configurations.

---

## 📌 API Functions

### `void gpio_init(GPIO_handle_t *pGPIOHandle)`

**Initialize a GPIO pin with user-defined configuration.**

This function configures:
- Pin mode (input, output, alternate function, analog)
- Output speed
- Output type (push-pull or open-drain)
- Pull-up/pull-down configuration
- Alternate function (if applicable)
- Interrupt settings (if enabled)

**Parameters**:
- `pGPIOHandle`: Pointer to a `GPIO_handle_t` structure containing:
  - `pGPIOX`: GPIO port (e.g., `GPIOA`, `GPIOB`)
  - `gpio_conf`: GPIO configuration settings

**Returns**: None

---

### `void gpio_deinit(GPIO_RegDef_t *pGPIOx)`

**Reset the entire GPIO port to default state.**

This function disables the peripheral clock and resets all configuration registers for the specified port.

**Parameters**:
- `pGPIOx`: Pointer to the GPIO port (e.g., `GPIOA`, `GPIOB`)

**Returns**: None

---

### `uint8_t gpio_read_pin(GPIO_RegDef_t *pGPIOx, uint8_t pin_num)`

**Read input level of a GPIO pin.**

**Parameters**:
- `pGPIOx`: Pointer to GPIO port
- `pin_num`: Pin number (0–15)

**Returns**: `0` (low) or `1` (high)

---

### `uint16_t gpio_read_port(GPIO_RegDef_t *pGPIOx)`

**Read 16-bit input level of entire GPIO port.**

**Parameters**:
- `pGPIOx`: Pointer to GPIO port

**Returns**: 16-bit input value

---

### `void gpio_write_pin(GPIO_RegDef_t *pGPIOx, uint8_t pin_num, uint8_t val)`

**Write output level to a GPIO pin.**

**Parameters**:
- `pGPIOx`: Pointer to GPIO port
- `pin_num`: Pin number
- `val`: Value to write (`0` or `1`)

**Returns**: None

---

### `void gpio_write_port(GPIO_RegDef_t *pGPIOx, uint16_t val)`

**Write 16-bit output value to entire GPIO port.**

**Parameters**:
- `pGPIOx`: Pointer to GPIO port
- `val`: 16-bit data to output

**Returns**: None

---

### `void gpio_toggle_pin(GPIO_RegDef_t *pGPIOx, uint8_t pin_num)`

**Toggle the output level of a pin.**

**Parameters**:
- `pGPIOx`: Pointer to GPIO port
- `pin_num`: Pin number to toggle

**Returns**: None

---

### `void gpio_peri_clk_ctrl(GPIO_RegDef_t *pGPIOx, uint8_t en_di)`

**Enable or disable peripheral clock for a GPIO port.**

**Parameters**:
- `pGPIOx`: GPIO port
- `en_di`: 1 to enable, 0 to disable

**Returns**: None

---
## `void IRQ_en(uint8_t irq_num, uint8_t priority, uint8_t set_clear)`

**Enable or disable an interrupt request (IRQ) in the Nested Vectored Interrupt Controller (NVIC) and set its priority.**

### Parameters:
- `irq_num`: The IRQ number to be configured.
- `priority`: The priority of the interrupt (0 is the highest priority, and 255 is the lowest priority).
- `set_clear`: A value of `1` enables the IRQ, and `0` disables the IRQ.

### Returns:
- `None`

### Note:
This function configures the interrupt priority and enables or disables the IRQ in the NVIC. The `irq_num` corresponds to the specific interrupt, `priority` determines the interrupt's priority level, and `set_clear` specifies whether to enable or disable the interrupt.


## 🧱 Data Structures

### `gpio_conf_t`

GPIO configuration structure.

```c
typedef struct {
    uint8_t pin_num;       // GPIO pin number (0-15)
    uint8_t mode;          // Mode: input, output, alt function, analog
    uint8_t out_speed;     // Output speed: low/medium/fast/high
    uint8_t out_type;      // Output type: push-pull or open-drain
    uint8_t pull_up_down;  // Pull-up/Pull-down configuration
    uint8_t alt_fun;       // Alternate function number (0–15)
    uint8_t irq_en;        // Enable interrupt (1 = enable, 0 = disable)
    uint8_t irq_mode;      // IRQ mode: rising, falling, both edges
} gpio_conf_t;
```

---

### `GPIO_handle_t`

GPIO handle structure to pass pin + config to APIs.

```c
typedef struct {
    GPIO_RegDef_t *pGPIOX;   // GPIO port base address
    gpio_conf_t gpio_conf;   // Pin configuration
} GPIO_handle_t;
```

---

## 🧾 Enumerations and Macros

### Pin Modes

```c
typedef enum
{
    GPIO_MODE_IN = 0,
    GPIO_MODE_OUT = 1,
    GPIO_MODE_ALT = 2,
    GPIO_MODE_ANALOG = 3,
    GPIO_MODE_IR = 4,
    GPIO_MODE_IF = 5,
    GPIO_MODE_IFR = 6,
}GPIO_MODE_t;
```

### Output Types

```c
typedef enum
{
    PUSH_PULL = 0x00,
    OPEN_DRAIN = 0x01,
} output_type_t;

```

### Output Speeds

```c
typedef enum
{
    LOW_SPEED = 0x00,
    MEDIUM_SPEED = 0x01,
    HIGH_SPEED = 0x02,
    VHIGH_SPEED = 0x03,
} out_type_t;

```
### INTERRUPT CONFIG

```c
typedef enum {
    INT_SET = 0,
    INT_CLEAR = 1,
    INT_PEND_SET = 2,
    INT_PEND_CLEAR = 3,
    INT_ACT_BIT = 4,
}INT_CONFIG_t;

```

### Pull-up / Pull-down

```c
typedef enum
{
    NO_PULL_PUSH = 0x00,
    PULL_UP      = 0x01,
    PULL_DOWN    = 0x02,
    RESERVE      = 0x03
} push_pull_t;

```

### ALTERNATE FUNCTIONS

```c
typedef enum 
{
    AF0 = 0,
    AF1 = 1,
    AF2 = 2,
    AF3 = 3,
    AF4 = 4,
    AF5 = 5,
    AF6 = 6,
    AF7 = 7,
    AF8 = 8,
    AF9 = 9,
    AF10 = 10,
    AF11 = 11,
    AF12 = 12,
    AF13 = 13,
    AF14 = 14,
    AF15 = 15
}alt_fun_t;

```




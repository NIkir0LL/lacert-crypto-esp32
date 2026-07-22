# lacert-crypto-esp32

[English](#english) · [Русский](#русский)

---

## English

**LACERT protocol client for ESP32, in C (ESP-IDF).**

This is the device-side firmware of LACERT — the post-quantum, cloud-free
scheme for connecting IoT devices to corporate networks. It runs the same
protocol as the Go gateway ([lacert-crypto-go](https://github.com/NIkir0LL/lacert-crypto-go)):
post-quantum handshake, continuous key rotation and remote firmware-integrity
checks — on constrained ESP32 hardware.

Tested on **ESP32-C6** (Seeed XIAO) and **ESP32-S3** (DevKitC-1).

### What is in this repository

Only the **original LACERT code**:

| Path | What |
|------|------|
| `main/lacert_client.c/.h` | protocol client (connect, telemetry, rotation) |
| `main/lacert_crypto.c/.h` | crypto glue (ML-KEM, BLAKE3, ChaCha20) |
| `main/lacert_wire.c/.h`   | message serialization |
| `main/main.c`             | Wi-Fi bring-up and the demo loop |
| `CMakeLists.txt`, `partitions.csv`, `sdkconfig.defaults*` | build config |

### Third-party components are **not** bundled

The cryptographic primitives themselves come from established projects and are
**not** copied into this repository:

- **ML-KEM-1024** — from [PQClean](https://github.com/PQClean/PQClean) (public domain)
- **BLAKE3** — the [reference C implementation](https://github.com/BLAKE3-team/BLAKE3) (CC0 / Apache-2.0)

Fetch them into `components/` before building:

```bash
./scripts/fetch-components.sh
```

### Build & flash

```bash
# 1. get the third-party crypto components
./scripts/fetch-components.sh

# 2. select your target and build (ESP-IDF v5.x)
idf.py set-target esp32c6      # or esp32s3
idf.py build

# 3. flash (XIAO → /dev/ttyACM0, DevKitC → /dev/ttyUSB0)
idf.py -p /dev/ttyACM0 flash monitor
```

> When you reflash with a changed binary, its SHA-256 changes too — remove the
> device record from the gateway database before re-enrolling, otherwise the
> integrity check will (correctly) reject it.

### License

Apache License 2.0 for the original code — see [LICENSE](LICENSE). The fetched
third-party components keep their own licenses.

---

## Русский

**Клиент протокола LACERT для ESP32 на языке C (ESP-IDF).**

Это прошивка для устройства в схеме LACERT — постквантовой, без облака, системе
безопасного подключения IoT-устройств к корпоративным сетям. Прошивка выполняет
тот же протокол, что и Go-шлюз ([lacert-crypto-go](https://github.com/NIkir0LL/lacert-crypto-go)):
постквантовое рукопожатие, непрерывную ротацию ключей и удалённую проверку
целостности прошивки — на устройстве с ограниченными ресурсами.

Проверено на **ESP32-C6** (Seeed XIAO) и **ESP32-S3** (DevKitC-1).

### Что лежит в репозитории

Только **оригинальный код LACERT**:

| Путь | Что это |
|------|---------|
| `main/lacert_client.c/.h` | клиент протокола (подключение, телеметрия, ротация) |
| `main/lacert_crypto.c/.h` | связка с криптографией (ML-KEM, BLAKE3, ChaCha20) |
| `main/lacert_wire.c/.h`   | сериализация сообщений |
| `main/main.c`             | поднятие Wi-Fi и демонстрационный цикл |
| `CMakeLists.txt`, `partitions.csv`, `sdkconfig.defaults*` | конфигурация сборки |

### Чужие компоненты **не хранятся** в репозитории

Сами криптографические примитивы взяты из известных проектов и **не** копируются
сюда:

- **ML-KEM-1024** — из [PQClean](https://github.com/PQClean/PQClean) (public domain)
- **BLAKE3** — [референсная реализация на C](https://github.com/BLAKE3-team/BLAKE3) (CC0 / Apache-2.0)

Перед сборкой их нужно подтянуть в `components/`:

```bash
./scripts/fetch-components.sh
```

### Сборка и прошивка

```bash
# 1. получить чужие криптокомпоненты
./scripts/fetch-components.sh

# 2. выбрать плату и собрать (ESP-IDF v5.x)
idf.py set-target esp32c6      # или esp32s3
idf.py build

# 3. прошить (XIAO → /dev/ttyACM0, DevKitC → /dev/ttyUSB0)
idf.py -p /dev/ttyACM0 flash monitor
```

> При перепрошивке изменённым бинарником меняется и его SHA-256 — перед
> повторной регистрацией удалите запись устройства из базы шлюза, иначе проверка
> целостности (правильно) его отклонит.

### Лицензия

Apache License 2.0 для оригинального кода — см. [LICENSE](LICENSE). Подтянутые
чужие компоненты сохраняют свои лицензии.

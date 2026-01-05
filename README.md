# SMIP Manager Configuration

* (Refer to the official CLI guide for full details:
https://www.analog.com/media/en/reference-design-documentation/design-notes/smartmesh_ip_embedded_manager_cli_guide.pdf
)

## 1. Access CLI

- Connect to the **SMIP Manager Eval Board** via USB
- CLI appears on the **3rd COM port**
- Serial settings:
    - **9600 baud**, 8N1, no flow control

## SMIP Manager Configuration

*(Ref: SmartMesh IP Embedded Manager CLI Guide)*

### 1. Access CLI
- Connect to the **SMIP Manager Eval Board** via USB
- CLI appears on the **3rd COM port**
- Serial settings:
    - **9600 baud**, 8N1, no flow control
```
> login user
```
### 2. Basic Network Configuration
```
> set config netid=1229              # 0–65535, must match motes
> delete acl all
> set config commonjoinkey=000102030405060708090A0B0C0D0E0F
> reset
```
### 3. Check Network Status
```
> sm           # show joined motes
> show time    # UTC time, system ticks, ASN
```
### 4. Useful Command
```
> set config <param>=<value>
```
(See CLI Guide, Page 34)

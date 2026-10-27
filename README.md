# Como desabilitar e reativar uma tecla no Ubuntu

Método: remapeamento via `udev/hwdb`  
Exemplo: tecla F7, código físico `41`

## 1. Identificar a tecla

Instale e execute o `evtest`:

```bash
sudo apt install evtest -y
sudo evtest
```

Selecione o teclado interno e pressione a tecla que deseja desabilitar.

Identifique os valores `MSC_SCAN` e `KEY`.

Exemplo para F7:

```text
MSC_SCAN = 41
KEY_F7 = 65
```

O código utilizado no remapeamento será o `MSC_SCAN`.

## 2. Desabilitar a tecla

### Criar o arquivo de configuração

```bash
sudo nano /etc/udev/hwdb.d/90-disable-key.hwdb
```

### Inserir a configuração

```text
evdev:atkbd:*
 KEYBOARD_KEY_41=reserved
```

Substitua `41` pelo código `MSC_SCAN` da tecla desejada.

**Importante:** mantenha um espaço antes de `KEYBOARD_KEY_41`.

### Salvar

`Ctrl + O` → `Enter` → `Ctrl + X`

### Aplicar e reiniciar

```bash
sudo systemd-hwdb update
sudo udevadm trigger
sudo reboot
```

## 3. Reativar a tecla

### Remover a configuração

```bash
sudo rm /etc/udev/hwdb.d/90-disable-key.hwdb
```

### Atualizar e reiniciar

```bash
sudo systemd-hwdb update
sudo udevadm trigger
sudo reboot
```

A tecla voltará ao comportamento original.

---

**Observações:**
- O procedimento é permanente até a remoção da configuração.
- Funciona no Wayland e no X11.
- A regra apresentada é específica para teclados gerenciados pelo driver `atkbd`.
- Para outros teclados, pode ser necessário adaptar a identificação do dispositivo.

# Plasma Treatment Machine HMI – 360 × 360

Obsah balíčku:
- `plasma_hmi.yaml` – LVGL obrazovky, navigace, dynamické popisky a dotykové oblasti.
- `assets/` – devět pozadí 360 × 360 px.
- `preview.png` – náhled všech obrazovek.

## Vložení do projektu ESPHome

1. Zkopíruj `plasma_hmi.yaml` a složku `assets` do stejné složky jako hlavní YAML zařízení.
2. Do hlavního YAML přidej:

```yaml
packages:
  plasma_hmi: !include plasma_hmi.yaml
```

3. Ve své konfiguraci displeje ponech:
```yaml
auto_clear_enabled: false
update_interval: never
```
a nepoužívej u displeje vlastní `lambda:`.

4. Konfigurace desky, displeje a touch kontroléru zůstává v hlavním YAML.

## Co už funguje

- splash screen a automatický přechod do menu,
- přechody mezi devíti obrazovkami,
- neviditelné dotykové oblasti s odezvou při stisku,
- volba receptury,
- zobrazení parametrů automatického cyklu,
- progress bar,
- ruční tlačítka s akcí `on_press` / `on_release`,
- nastavení jazyka, jasu, výkonu plasmy, prodlevy a jednotek,
- alarmová obrazovka,
- informační obrazovka.

## Napojení na logiku stroje

V souboru vyhledej `TODO`. Tyto skripty jsou připravené pro napojení na skutečné řízení:
- `machine_start_request`
- `machine_stop_request`
- `machine_home_all`
- `jog_x_positive_start`
- `jog_x_negative_start`
- `jog_y_positive_start`
- `jog_y_negative_start`
- `jog_all_stop`

Aktualizace průběhu cyklu:

```yaml
- script.execute:
    id: ui_set_progress
    value: 62
```

Zobrazení alarmu:

```yaml
- script.execute:
    id: ui_show_alarm
    title: "HOME SENSOR ERROR"
    message: "X AXIS HOME SENSOR\nNOT DETECTED"
```

## Bezpečnost

Dotykový displej ani ESP32 nesmí být jediným bezpečnostním prvkem stroje. Nouzové zastavení, blokování dveří/krytu, vypnutí vysokého napětí plasmy a bezpečné zastavení pohonů musí být řešeno hardwarově a nezávisle na HMI.

<<<<<<< HEAD
# Jubilee_KTCC_Config
Backup of the klipper config on my Jubilee Toolchanger


Prusaslicer Custom GCode:

Start:
```
SET_PRINT_STATS_INFO TOTAL_LAYER=[total_layer_count]

KTCC_INIT_PRINT_STATS

M140 S[first_layer_bed_temperature] ; Heat the bed first

M116 H0 S2 ; Wait for bed to reach temperature with 2 degrees tolerance

M568 P[initial_extruder] R{idle_temperature[initial_extruder]} S{first_layer_temperature[initial_extruder]} A1

G28

BED_MESH_CALIBRATE

G0 Z3 F5000	; Ensure nozzle is at 3mm over the bed

SAVE_POSITION X={first_layer_print_max[0]} Y={first_layer_print_min[1]}

T[initial_extruder]	; Mount extruder first used (even if only one extruder used). Waits for temperature inside the script.

G0 X{first_layer_print_max[0]} Y{first_layer_print_min[1]} Z3 F30000
```


End:
```
; cooldown
M104 S0

; turn off extruders
G10 P0 S0 R0 A0	; extruder 0
;G10 P1 S0 R0 A0	; extruder 1
;G10 P2 S0 R0 A0	; extruder 2

; turn off bed
M140 S0

; dropoff current tool
T_1

; move bed down another 20mm
G91
G0 Z20
G90

; park toolhead
G0 X1 Y1 F30000

; Reset saved position
SAVE_POSITION

; turn off motors
M84

; Print toolchanger extension statistics to console
KTCC_DUMP_PRINT_STATS
```

After Layer Change:
```
SET_PRINT_STATS_INFO CURRENT_LAYER={layer_num + 1}
```

Toolchange:
```
{if layer_num < 2}

;First layer temperature for next extruder
M568 P[next_extruder] R{idle_temperature[next_extruder]} S{first_layer_temperature[next_extruder]} A2 

{else}

;Other layer temperature for next extruder
M568 P[next_extruder] R{idle_temperature[next_extruder]} S{temperature[next_extruder]]} A2 

{endif}
T{next_extruder}
```
=======
# Jubilee KTC Config
Config for my Jubilee Toolchanger using the KTC toolchanger extension for Klipper.
>>>>>>> b1e5a5f3a47bc99cfc030d69eaeae5289cd085a0

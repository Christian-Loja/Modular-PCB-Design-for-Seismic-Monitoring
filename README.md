# 🚀 PCB para Monitoreo de Señales Acelerométricas en Red de Control Sísmico  

## 📌 Introducción  
Este proyecto consiste en el diseño de una placa PCB con componentes y sensores modulares para la adquisición y transmisión de señales acelerométricas, destinada a redes de control sísmico. La placa integra un microcontrolador **ESP32** para procesamiento y comunicación inalámbrica, junto con componentes electrónicos y sensores optimizados para garantizar precisión en la medición de vibraciones.  

---

## 🔍 Componentes Utilizados  
A continuación se detallan los componentes clave del diseño:  

### 🎛️ Microcontrolador  
- **ESP32-WROOM-32** (Modelo: `ESP32-WROOM-32E`, Tipo: `SMD`).  

### 📊 Sensores, Módulos y Reloj
- **Acelerómetro** (Modelo: `ADXL355Z` o equivalente, Tipo: `Modular`).
- **GPS** (Modelo: `GPS-FPGMMOPA6H` o equivalente, Tipo: `Modular`).
- **RTC** (Modelo: `DS3231` o equivalente, Tipo: `SMD SOIC-16`).  

### 🔧 Circuitos Integrados (ICs)  
- **LD33V** (Tipo: `SMD SOT-223` o equivalente). 

#### Capacitores  
- `C1`: 100nF (Tipo: `Cerámico Modular 2.54mm`, Código: `104`).
- `C2`: 10µF (Tipo: `Electrolítico Modular 5mm`).
- `C3`: 10µF (Tipo: `Electrolítico Modular 5mm`).
- `C4`: 100nF (Tipo: `Cerámico Modular 2.54mm`, Código: `104`).
- `C5`: 100µF (Tipo: `Electrolítico Modular 5mm`).
- `C6`: 100µF (Tipo: `Electrolítico Modular 5mm`).
- `C7`: 100nF (Tipo: `Cerámico Modular 2.54mm`, Código: `104`).

### 🔋 Conector de Fuente de Alimentación (+12V)  
- Conector XH 2 posiciones.  

### 📊 Test Points para Depuración  
- Pin Header 1x2 (Conectado a LD33V): `GND/VCC`
- Pin Header 1x2 (Conectado a DS3231): `SDA1/SCL1`
- Pin Header 1x2 (Conectado a GPS): `TXGPS/RXGPS`
- Pin Header 1x4 (Conectado a Lector Tarjeta µSD): `CS/SCK/MOSI/MISO`
- Pin Header 1x4 (Conectado a ADXL355Z): `IO15/IO13/IO12/IO14`

---

## 🖼️ Imágenes del Diseño  

### Esquemático  
![Esquemático](/images_schematic)  

### Circuito Impreso  
![PCB](/images_PCB_layers)  

### Vista 3D de la PCB  
![Vista 3D de la PCB](/images_PCB_3D_view)  

---

## 📥 Descarga de Archivos Gerber, ODB, Drill y BOM
Los archivos para fabricación están disponibles en la carpeta [`/PCB_Modular`](/PCB_Modular) de este repositorio. Los archivos Gerber se pueden visualizar en el archivo PDF ![Gerber images](/PCB_Modular/CAMOutputs/Gerber_images.pdf).

---

## 🛠️ Características y Sustento del Diseño 
Este proyecto se ha realizado con el objetivo de optimizar el prototipo desarollado sobre Protoboard para el sistema de monitoreo sísmico, para esto se han recopilado los componentes, en su mayoría modulares, necesarios para el proyecto. El obejtivo de esta placa es facilitar la depuración del sistema, reducir el tamaño del prototipo y mejorar la comunicación entre módulos mediante pistas de cobre con menor ruido. Adicionalmente, se han implementado zonas de cobre y planos de masa para minimizar el ruido e interferencias y mejorar la disipación térmica. Se han colocado varios puntos de prueba (test points) para depuración a partir de peinetas (Pin Headers macho) en donde se pueden conectar jumpers o sondas de osciloscopio para observar las señales provenientes del módulo GPS, módulo RTC, acelerómetro, tarjeta micro SD y los voltajes de alimentación que se dirigen al ESP32 y demás componentes.

En la etapa de alimentación se ha implementado una fuente tipo Buck-Converter junto a un conjunto de capacitores que filtrarán el ruido de alta frecuencia para mantener el nivel de voltaje de salida estable. Los capacitores utilizados son electrolíticos y cerámicos modulares para mejorar la disipación de potencia y también debido a que los valores de capacitancia y voltaje requeridos son dificiles de conseguir en una versión SMD (se requieren capacitores de tantalio). 

### ✔️ Características Clave  
- **Reducción de tamaño**: Optimización del espacio en la placa al utilizar el espacio requerido por los módulos ESP32DEV-KIT, Buck-Converter MH-Mini360, Lector µSD y RTC-DS3231. La implementación de los módulos sobre una PCB proporciona mayor resistencia al movimiento y vibraciones a comparación con sus versiones modulares sobre Protoboards. 
- **Bajo ruido**: Para minimizar interferencias en las señales se ha colocado un plano de tierra en ambas caras del diseño, de modo que se cortocircuite cualquier señal no deseada que incida sobre la placa. Estos planos de tierra también mejoran la disipación térmica de los componentes, por ende, se reduce el ruido térmico.  
- **Comunicación inalámbrica**: Para mantener las funciones de conectividad Wifi se utilizó el DevKit de ESP32 con microcontrolador modelo ESP32-WROOM-32E cuyo encapsulado cuenta con una antena externa para la comunicación.  
- **Robustez y eficiencia energética**: Para asegurar la integridad de los componentes presentes y un consumo eficiente de la energía se ha implementado una fuente de voltaje con Buck-Converter que transforma el voltaje de entrada de +12V a +5V. Este último valor de voltaje es convertido a +3.3V usando un regulador de voltaje LD33V (este voltaje resultante alimentará todo el sistema). La fuente de alimentación principal se ha implementado mediante un Buck-Converter debido a que permite manejar corrientes considerables manteniendo una generación de calor baja y, principalemte, debido a la alta eficiencia energética que este tipo de fuentes proporciona (aprox 85%).
  
### 📐 Criterios de Diseño  
- **Grosor de líneas de alimentación**: Para una corriente de `1A` se recomienda `1mm` de ancho de pista, por lo tanto, dependiendo de la ubicación de la pista se usarán grosores dentro del rango `0.5–1mm` ya que las corrientes que circulan por la placa están por debajo de 1A.  
- **Grosor de líneas de transmisión**: Para comunicación SPI se recomiendan grosores de pista dentro del rango `0.2–0.3mm`. Para comunicación I2C se recomienda el rango `0.15–0.25mm`. Para mantener lineas de un solo grosor se usará `0.2032mm` para SPI e I2C.
- **Diseño de doble cara**: Para facilitar la alimentación y conexión entre componentes se ha diseñado una placa de doble cara. La cara superior contiene todos los componentes y la mayoria de conexiones entre ellos, la cara inferior se usa para transportar señales de comunicación SPI o I2C que no puedan enrutarse en la capa superior. El diseño de doble cara permite tener un plano de tierra que cubra gran parte de ambas caras con esto se logra reducir significativamente las interferencias.
- **Separación entre líneas de transmisión**: Para evitar acoplamientos e interferencias en las señales de SPI se recomienda una distancia entre pistas de almenos 3 veces el ancho de la pista. De forma similar para las señales de I2C se recomienda una separación de almenos 2 veces el ancho de pista. Estos parámetros fueron tomados en cuenta en todos los enrutamientos de la placa.

---

## 🎯 Conclusiones y Recomendaciones  
### ✅ Verificación de errores eléctricos (ERC) y Verificación de reglas de diseño (DRC).
- La validación de reglas de diseño y de errores eléctricos realizada por el software Fusion 360 se muestra en una tabla que lista los errores encontrados. En este caso la validación de ERC y DRC muestran cero errores, estos resultados se muestran en las carpetas ![Esquemático](/images_schematic) y ![PCB](/images_PCB_layers) respectivamente.

### ✅ Conclusiones  
- La placa cumple con los requisitos de precisión y escalabilidad para redes sísmicas.  
- El uso del ESP32 reduce costos y simplifica la integración con sistemas IoT.  

### ⚠️ Precauciones  
- **Montaje**: Verificar polaridad de componentes sensibles (ej. entrada de 12 voltios y capacitores electrolíticos).  
- **Adquisición de componentes**: Asegurar que los componentes por comprar tengan un codigo SMD correspondiente a cada elemento listado en este repositorio, caso contrario, no será posible soldarlos en la placa por la diferencia de dimensiones.  

### 🔮 Recomendaciones  
- Añadir una etapa de protección de sobretensiones y de pico de corrientes para el circuito, ya que el fusible colocado en el diseño protege unicamente de los aumentos de corriente.  
- Considerar encapsulado/blindaje para ambientes hostiles.
- Añadir un circuito para alimentación de respaldo para todo el sistema. Actualmente solo el RTC DS3231 cuenta con una bateria que mantiene activo el reloj durante periodos de desconexión eléctrica.

---

## 📜 Licencia  
Este proyecto está bajo licencia **MIT**. Para más detalles, consulta el archivo [LICENSE](/LICENSE).  

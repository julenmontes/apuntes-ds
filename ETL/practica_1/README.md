 
Pasos a realizar:  
1) Instalar python  
 Recordar incorporar python como variable de entorno (se puede hacer en la propia instalación
2) Intalar virtualenv:  
```
python -m pip install --user virtualenv
```
3) Creamos el entorno virtual y lo llamamos 'dev_practicas' 
   >(`py` es equivalente a `python`)  
```
py -m venv dev_practicas
```
y lo activamos:
```
.\dev_practicas\Scripts\activate  
```
4) instalar los paquetes que vamos a utilizar
```
py -m pip install numpy
py -m pip install pandas
```
 
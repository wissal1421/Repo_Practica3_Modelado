# Análisis y Teleoperación de Plataforma Móvil con Brazo Robótico

## Objetivo
El objetivo de esta práctica es hacer que un robot movil de 6 ruedas se mueva mediante teleoperación hasta el lugar donde se encuentra una caja, plaanifique todos los movimientos que tengan que ver con el brazo Scara y el gripper que sujeta objeto y, por último, generar graficas que demuestren que los topicos en los que se publica la informacion del robot sea correcta y podamos analizarla.

## Descripción del robot
* Base móvil con 6 ruedas omnidireccionales.

* Brazo tipo SCARA con 3 articulaciones y gripper de 4 dedos.

* Sensores integrados: cámara frontal, cámara en gripper y sensor IMU en la base(esta justo en el ceentro del robot por lo que no puede visualizarse directamente).

## Pasos para realizar la simulacion
### Paso 1
* Lanzamos `robot_gazebo.launch.py` para poder ver tanto el mundo en gazebo con el robot como la informacion de dicho robot en rviz.
* Tras comprobar que se lanzo correctamente el primer launcher lanzamos `rover_moveit_config move_group.launch.py`, con el que se lanza el nodo principal de MoveIt.
* Una vez tengamos los dos apartados anteriores bien, se lanza el cotrolador del robot, al añadir este es cuando se observa correctamente el robot en rviz.
* Lanzaremos antes de empezar a teleoperar un rosbag que capture todo lo que pase en los topicos: `/imu` y `/joint_states`.

### Paso 2
Con el paso anterior completado, podeos iniciar a teleoperar el robot, se introduce el siguiente comando en una terminal:
```bash
ros2 run teleop_twist_keyboard teleop_twist_keyboard
```
Al lanzar esto nos sale una zona en la que podemos teleoperar el robot mediante teclas del ordenador, asi como aumentar o disminuir la velocidad angular o lineal, teniendo esto movemos el robot hasta que el gripper quede justo encima de la caja. Debo añadir que en mi caso no es necesario planificar nada hasta este momento porque la posicion inicial de mi robot es con la pinza abierta y el brazo en alto.

### Paso 3
Una vez tenemos el robot en la posicion adecuada, comienza la planificación!!! Primero accederemos a la ventana de rviz que el launcher del gazebo nos abrio, seleccionamos en el apartado add la seccion que indica `MotionPlanning`, una vez abierta esta sección selecionamos en el contexto la opion `OMPL` y planificamos y ejecutamos todas las veces que sea necesario tanto el gripper como el brazo para hacer que la caja sea elevada del suelo y llevada al compartimento que le corresponde en la parte de atras del robot.

### Paso 4
Como hemos terminado lo que queriamos analizar, paramos el rosbag que lanzamos en el paso 1, y todos los demas launchers ya que no seran necesarios más, y procederemos a analizar los topicos. Para hacer esto lanzaremos en una terminal `plotjuggler` y cargaremos en el el rosbag, introduciremos lo que nos interesa de cada topic, pulsamos el botón `start` y veremos que aparece en la gráfica los datos de cada cosa que hemos añadido.

## Análisis de las gráficas
### Gráfico Tiempo vs G-parcial (Esfuerzo por articulación del brazo)
Este gráfico representa el esfuerzo (effort) aplicado por los motores de las articulaciones del brazo y la pinza durante todo el proceso. Se incluyen las articulaciones:
* Brazo_l_1_link_joint
* Brazo_l_2_link_joint
* Brazo_l_3_link_joint
* Base_pinza_link_joint

![Gráfico de esfuerzo](https://github.com/wissal1421/Repo_Practica3_Modelado/blob/78fd7797d6cdf2879b641041a613444d2578617e/grafico_gasto.png)

Se puede observar: 
* El incremento de esfuerzo entre los segundos 10 y 50 coincide con la bajada del brazo hacia la caja.

* Entre los segundos 50 y 90 se observan picos bruscos, correspondientes al agarre y elevación de la caja, especialmente en Brazo_l_3_link_joint (eje prismatic).

* A partir del segundo 100, el esfuerzo disminuye de forma estable, indicando la finalización de la operación y reposicionamiento del brazo.

### Gráfico Tiempo vs Posición de cada una de las ruedas
Este gráfico refleja el desplazamiento de las 6 ruedas (3 por cada lateral) a lo largo del tiempo, obtenidas del topic /joint_states.

![Gráfico de posición](https://github.com/wissal1421/Repo_Practica3_Modelado/blob/78fd7797d6cdf2879b641041a613444d2578617e/grafico_pos_ruedas.png)

Se puede observar:

* El movimiento comienza al inicio de la teleoperación (~segundo 0) y continúa hasta aproximadamente el segundo 60.

* Se aprecia un incremento suave y sincronizado en las posiciones, lo que indica una conducción recta hacia el objetivo.

* La estabilización de las curvas a partir del segundo 60 refleja que el robot se detiene mientras el brazo realiza el pick and place.

### Gráfico Tiempo vs Aceleración lineal (IMU)
Este gráfico muestra la aceleración lineal en los ejes X, Y y Z a partir del sensor IMU.

![Gráfico de aceleración](https://github.com/wissal1421/Repo_Practica3_Modelado/blob/78fd7797d6cdf2879b641041a613444d2578617e/grafico_ac_ruedas.png)

Se puede observar:

* Las aceleraciones en x y y presentan variaciones notables durante el desplazamiento, con oscilaciones que reflejan pequeños ajustes de velocidad del robot.

* En z, se observan perturbaciones más marcadas al momento de bajar y subir la pinza (entre segundos 40 y 100), probablemente causadas por la vibración inducida por los movimientos verticales del brazo.

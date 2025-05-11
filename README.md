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

## Paso 2
Con el paso anterior completado, podeos iniciar a teleoperar el robot, se introduce el siguiente comando en una terminal:
```bash
ros2 run teleop_twist_keyboard teleop_twist_keyboard
```
Al lanzar esto nos sale una zona en la que podemos teleoperar el robot mediante teclas del ordenador, asi como aumentar o disminuir la velocidad angular o lineal, teniendo esto movemos el robot hasta que el gripper quede justo encima de la caja. Debo añadir que en mi caso no es necesario planificar nada hasta este momento porque la posicion inicial de mi robot es con la pinza abierta y el brazo en alto.

## Paso 3

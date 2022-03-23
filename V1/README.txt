EJERCICIOS REALIZADOS PARA ESTA ENTREGA:
	- Ejercicio 0: Implementación de la instrucción MEMADD (creación de un nuevo case dentro de Processon.c y modificación del fichero Instructions.def).
		./Simulator prog-V1-E0
	- Ejercicio 1 y 2: Implementación de la función Computer_System_PrintProgramList en ComputerSystem.c y colocar una llamada a dicha función en Simulator.c junto con la modificación del fichero messagesSTD.txt.
	- Ejercico 3: Implementación de un programa similar a 'programVerySimple' que termina con la instrucción TRAP 3 (no TRAP 5), que solicita la finalización del sistema operativo.
		./Simulator --debugSections=HD prog-V1-E3 prog-V1-E3
	- Ejercicio 4: Implementación de un if en la función OperatingSystem_CreateProcess para tratar el caso de que se introduzca un número de programas mayor que la cantidad de entradas de la tabla de procesos disponibles, implementación de un switch-case en la función OperatingSystem_LongTermScheduler para comprobar ese caso junto con la modificación del fichero messagesSTD.txt.
		./Simulator programVerySimpleTrap programVerySimpleTrap programVerySimpleTrap prog-V1-E4
			nótese que prog-V1-E4 es idénticamente igual al programa programVerySimpleTrap, con un nombre diferente para que se vea que es el último proceso el que no se puede crear.
	- Ejercicio 5: Implementación de dos nuevos if en la función OperatingSystem_CreateProcess para tratar el caso de que el programa introducido no exista o la prioridad o el tamaño sean inválidos, implementación de dos nuevos case en la función OperatingSystem_LongTermScheduler para tratar dichos casos y modificación del fichero messagesSTD.txt.
		./Simulator nonexistingProgram | hard -n 50
		./Simulator prog-V1-E5 | head -n 50
			nótese que nonexistingProgram no existe, por lo que no se encuentra presente en el directorio, se usa un pipe y la instrucción head -n 50 para evitar que imprima líneas del systemIdleProcess infinitamente.
	- Ejercicio 6: Implementación de un nuevo if en la función OperatingSystem_CreateProcess para tratar el caso de que el programa introducido tenga un tamaño mayor que el tamaño designado para un programa de usuario, implementación de un nuevo case en la función OperatingSystem_LongTermScheduler para tratar dicho caso y modificación del fichero messagesSTD.txt.
		./Simulator prog-V1-E6 | head -n 50
	- Ejercicio 7: Implementación de un nuevo if en la función OperatingSystem_CreateProcess para tratar el caso de que el programa introducido tenga un tamaño mayor que el tamaño especificado dentro del código del programa.
		./Simulator prog-V1-E7 | head -n 50
	- Ejercicio 8: Modificación del archivo OperatingSystem.c para que el valor de initialPID sea por defecto la última posición libre de la tabla de procesos (en la versión actual 3).
		./Simulator programVerySimpleTrap
		./Simulator --initialPID=0 programVerySimpleTrap
			nótese que con la primera instrucción el systemIdleProcess se carga en la posición 3 por defecto y que con la segunda se carga de manera forzosa en la posición 0
	- Ejercicio 9: Implementación de la función OperatingSystem_PrintReadyToRunQueue, que imprime todos los procesos listos para ser ejecutados. LLamada a dicha función dentro de la función OperatingSystem_MoveToTheREADYState. Junto con el archivo messagesSTD.txt
		./Simulator programVerySimpleTrap
	- Ejercicio 10: Modificación del archivo OperatingSystem.c para implementar el array statesNames. Llamada a la función OperatingSystem_DebugMessage en las funciones OperatingSystem_PCBInitialization, OperatingSystem_MoveToTheREADYState, OperatingSystem_Dispatch, OperatingSystem_TerminateProcess para imprimir los cambios de estado. Junto con el archivo messagesSTD.txt.
		./Simulator programVerySimpleTrap
	- Ejercicio 11: Modificación de OperatingSystem.h, OperatingSystem.c con extractos de código de la tarea. Modificación de las funciones OperatingSystem_PrintReadyToRunQueue, OperatingSystem_CreateProcess, OperatingSystem_PCBInitialization, OperatingSystem_MoveToTheREADYState, OperatingSystem_ExtractFromReadyToRun para dar soporte a las dos colas de prioridad (user y daemon).
		./Simulator programVerySimple programVerySimpleTrap
	- Ejercicio 12: Modificación del case para el SYSCALL_YIELD, nuevo mensaje en el fichero messagesSTD.txt y creación de programas para probar los casos.
		./Simulator prog-V1-E12a prog-V1-E12b prog-V1-E12c
	- Ejercicio 13:
		a) ¿Por qué hace falta salvar el valor actual del registro PC del procesador y de la PSW?
			Es necesario guardar los valores del registro PC y PSW para, al retomar la ejecucicón normal del sistema operativo, poder saber qué instrucción hay que ejecutar y cuál era el estado de la máquina, estas informaciones se registran en el PC y PSW respectivamente.
		b) ¿Sería necesario salvar algún valor más?
			Sí, también es necesario guardar el registro acumulador, ya que se podía estar usando antes de tener que guardar el contexto.
		c) A la vista de lo contestado en los apartados a) y b) anteriores, ¿sería necesario realizar alguna modificación en la función OperatingSystem_RestoreContext()? ¿Por qué?
			Sí, para que se guarde el valor del acumulador y prosiga la ejecución de forma correcta del programa interrumpido.
		d) ¿Afectarían los cambios anteriores a la implementación de alguna otra función o a la definición de alguna estructura de datos?
			Sí, se ha de cambiar la estructura del PCB para guardar una copia del registro acumulador y las funciones OperatingSystem_PCBInitialization, OperatingSystem_RestoreContext, OperatingSystem_SaveContext.
		./Simulator prog-V1-E13a prog-V1-E13b
			* El valor del acumulador tras retomar la ejecución del prog-V1-E13a deberá volver a ser 10
	- Ejercicio 14: Cuando el último proceso es el systemIdleProcess, en concreto, cuando el último proceso no es de usuario, el PC pasa a estar sobre la instrucción HALT del sistema operativo, por lo que el simulador se apaga mendiante la asignación al procesador de un proceso que apaga el simulador.
	- Ejercicio 15: Modificación de la función OperatingSystem_Initialize para que si no se crean procesos válidos de usuario se llame a la función OperatingSystem_ReadyToShutdown para apagar el simulador.
		./Simulator nonexistingProgram
			nótese que nonexistingProgram no existe, por lo que no se encuentra presente en el directorio, es solo un nombre cualquiera para probar que el simulador se apaga cuando no hay procesos de usuario válidos a ejecutar.
	-Ejercicio 16: Modificación de la función Processor_DecodeAndExecuteInstruction para que en caso de que se ejecuten las instrucciones HALT, OS o IRET, se compruebe que el modo de ejecución es núcleo, en caso de que no lo sea, se lanza una excepción.
		./Simulator prog-V1-E16a prog-V1-E16b prog-V1-E16c

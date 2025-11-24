// Funcionamiento contadores
IF marcha_OFF=FALSE THEN
	CTU_1(CU:=marcha_1, RESET:=reset_1 OR reset_receta, PV:=contador_1_ajustable, Q=>completo_1, CV=>salida_contador_1);
	CTU_2(CU:=marcha_2, RESET:=reset_2 OR reset_receta, PV:=contador_2_ajustable, Q=>completo_2, CV=>salida_contador_2);
	CTU_3(CU:=marcha_3, RESET:=reset_3 OR reset_receta, PV:=contador_3_ajustable, Q=>completo_3, CV=>salida_contador_3);
	
	IF reset_1=TRUE THEN
		llenando_1:=FALSE;
		completo_1:=FALSE;
		vacio_1:=TRUE;
	END_IF;
	
	IF reset_2=TRUE THEN
		llenando_2:=FALSE;
		completo_2:=FALSE;
		vacio_2:=TRUE;
	END_IF;
	
	IF reset_3=TRUE THEN
		llenando_3:=FALSE;
		completo_3:=FALSE;
		vacio_3:=TRUE;
	END_IF;
	
ELSIF marcha_OFF=TRUE THEN
	vacio_1:=Parpadeo_leds;
	llenando_1:=Parpadeo_leds;
	completo_1:=Parpadeo_leds;
	vacio_2:=Parpadeo_leds;
	llenando_2:=Parpadeo_leds;
	completo_2:=Parpadeo_leds;
	vacio_3:=Parpadeo_leds;
	llenando_3:=Parpadeo_leds;
	completo_3:=Parpadeo_leds;
END_IF;

//Funcionamiento LEDS
IF CTU_1.CV=0 THEN
	vacio_1:=TRUE;
ELSIF CTU_1.CV<contador_1_ajustable THEN
	vacio_1:=FALSE;
	llenando_1:=TRUE;
ELSIF CTU_1.CV=contador_1_ajustable THEN
	llenando_1:=FALSE;
	completo_1:=TRUE;
END_IF;

IF CTU_2.CV=0 THEN
	vacio_2:=TRUE;
ELSIF CTU_2.CV<contador_2_ajustable THEN
	vacio_2:=FALSE;
	llenando_2:=TRUE;
ELSIF CTU_2.CV=contador_2_ajustable THEN
	llenando_2:=FALSE;
	completo_2:=TRUE;
END_IF;

IF CTU_3.CV=0 THEN
	vacio_3:=TRUE;
ELSIF CTU_3.CV<contador_3_ajustable THEN
	vacio_3:=FALSE;
	llenando_3:=TRUE;
ELSIF CTU_3.CV=contador_3_ajustable THEN
	llenando_3:=FALSE;
	completo_3:=TRUE;
END_IF;

//Funcionamiento reset_receta
IF reset_receta=TRUE THEN
	completo_1:=FALSE;
	llenando_1:=FALSE;
	vacio_1:=TRUE;
	completo_2:=FALSE;
	llenando_2:=FALSE;
	vacio_2:=TRUE;
	completo_3:=FALSE;
	llenando_3:=FALSE;
	vacio_3:=TRUE;
	contador_1_ajustable:=0;
	contador_2_ajustable:=0;
	contador_3_ajustable:=0;
END_IF;

//Temporizadores para el modo mantenimiento
TON_1(In:=marcha_OFF AND NOT reset_parpadeo, PT:=T#2s, Q=>Parpadeo_leds);
RESET_TON_1(In:=TON_1.Q, PT:=T#1s, Q=>reset_parpadeo);

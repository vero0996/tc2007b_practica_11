Verónica Paola Zapata Sánchez 
A01199193

Ejercicio 0:
1. Si todas usaran "ana", la segunda prueba fallaría al intentar registrarla debido a un error de conflicto (409) por usuario existente, además de fallar en posteriores ejecuciones de las pruebas.
2. Usar PostgreSQL real demuestra el comportamiento auténtico del servidor y sus permisos en producción, aunque se pierde velocidad de ejecución y exige tener Docker activo.

Ejercicio B4:
El bloque except IntegrityError solo se activa si dos peticiones con la misma clave se envían al mismo tiempo exacto; si se envían una después de otra, nunca fallará ahí. Para probarlo, se necesitan dos hilos ejecutándose a la vez usando una barrera para soltarlos juntos, y repetirlo varias veces. Esto se comprobó manualmente para la guía (lanzando cuatro envíos simultáneos 20 veces, donde PostgreSQL rechazó 57 duplicados y dejó exactamente 20 avisos), y el hecho de haberlo probado a mano en lugar de tener una prueba automatizada debe registrarse en la matriz del Bloque D.

Ejercicio D2:
El requisito de seguridad valida que solo el autor elimine sus publicaciones respondiendo 403 ante avisos ajenos y manteniéndolos en el tablón (tests/test_permisos.py::test_bruno_no_puede_borrar_el_aviso_de_ana, estado: Cumplido); el requisito funcional asegura que reintentar una publicación con la misma Idempotency-Key no cree duplicados al responder 201 y conservar una sola fila (tests/test_duplicados.py::test_reintentar_con_la_misma_clave_no_crea_otro_aviso, estado: Cumplido); mientras que el requisito de concurrencia busca rechazar solicitudes simultáneas con la misma clave mediante la restricción única de PostgreSQL, manteniéndose en estado Parcialmente cumplido (evidencia: Comprobación manual con 4 hilos simultáneos) dado que la suite de pruebas actual es secuencial y requiere una prueba automatizada con hilos en paralelo.
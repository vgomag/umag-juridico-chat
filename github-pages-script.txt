document.addEventListener('DOMContentLoaded', () => {
    const form = document.getElementById('consulta-form');
    const funcionesSelect = document.getElementById('funciones');
    const camposAdicionales = document.getElementById('campos-adicionales');
    const resultadoDiv = document.getElementById('resultado');
    const resultadoTexto = document.getElementById('resultado-texto');

    // Mapeo de funciones a campos adicionales
    const camposPorFuncion = {
        'redaccion_inicio': `
            <div class="mb-3">
                <label class="form-label">Número de Expediente</label>
                <input type="text" class="form-control" id="expediente" required>
            </div>
        `,
        'ley_karin': `
            <div class="mb-3">
                <label class="form-label">Detalles específicos de consulta</label>
                <textarea class="form-control" id="detalle-karin" rows="3"></textarea>
            </div>
        `,
        'vista_fiscal': `
            <div class="mb-3">
                <label class="form-label">Número de Caso</label>
                <input type="text" class="form-control" id="caso-fiscal" required>
            </div>
        `
    };

    funcionesSelect.addEventListener('change', (e) => {
        const funcionSeleccionada = e.target.value;
        camposAdicionales.innerHTML = camposPorFuncion[funcionSeleccionada] || '';
    });

    form.addEventListener('submit', async (e) => {
        e.preventDefault();

        const pregunta = document.getElementById('pregunta').value;
        const funcion = funcionesSelect.value;

        // URL de tu Cloud Function
        const CLOUD_FUNCTION_URL = 'https://tu-cloud-function-url.cloudfunctions.net/procesarConsulta';

        try {
            const response = await fetch(CLOUD_FUNCTION_URL, {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({ 
                    pregunta, 
                    funcion 
                })
            });

            const data = await response.json();

            resultadoDiv.style.display = 'block';
            resultadoTexto.textContent = data.respuesta || 'Sin respuesta disponible';
        } catch (error) {
            resultadoDiv.style.display = 'block';
            resultadoTexto.textContent = `Error: ${error.message}`;
        }
    });
});

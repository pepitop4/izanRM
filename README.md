<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Widget para StreamElements</title>
    <style>
        body {
            margin: 0;
            padding: 0;
            font-family: Arial, sans-serif;
            background-color: rgba(0, 0, 0, 0.5);
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }
        .widget-container {
            text-align: center;
            padding: 20px;
            background-color: rgba(255, 255, 255, 0.9);
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.3);
            position: relative;
        }
        .widget-image {
            max-width: 100%;
            border-radius: 10px;
            margin-bottom: 20px;
        }
        .overlay {
            position: relative;
            text-align: left;
            margin-top: 10px;
        }
        .overlay-item {
            font-size: 16px;
            background-color: rgba(0, 0, 0, 0.7);
            color: white;
            padding: 8px 12px;
            margin-top: 5px;
            border-radius: 5px;
            display: block;
        }
        .hidden {
            display: none;
        }
    </style>
</head>
<body>
    <div class="widget-container hidden" id="widget">
        <img class="widget-image" id="image" src="https://i.ibb.co/sp48znv/images-removebg-preview.png" alt="Imagen del Widget">
        <div class="overlay" id="overlay">
        </div>
    </div>

    <script>
        // Lista de atributos con porcentajes
        const attributes = [
            { name: 'Daño de media', min: -59, max: 59 },
            { name: 'Daño de habilidad', min: -25, max: 25 },
            { name: 'Fuerza contra Medio Humanos', values: ['1%', '5%', '10%'] },
            { name: 'Velocidad de hechizo', values: ['6%', '10%', '20%'] },
            { name: 'Fuerza contra Demonios', values: ['4%', '6%', '10%', '20%'] },
            { name: 'Fuerza contra Orcos', values: ['4%', '6%', '10%', '20%'] },
            { name: 'Fuerza contra Místicos', values: ['4%', '6%', '10%', '20%'] },
            { name: 'Fuerza contra Animales', values: ['4%', '6%', '10%', '20%'] },
            { name: 'Fuerza contra Nomuertos', values: ['4%', '6%', '10%', '20%'] },
            { name: 'Fuerza', values: ['1%', '4%', '6%', '8%', '12%'] },
            { name: 'Inteligencia', values: ['1%', '4%', '6%', '8%', '12%'] },
            { name: 'Destreza', values: ['1%', '4%', '6%', '8%', '12%'] },
            { name: 'Vitalidad', values: ['1%', '4%', '6%', '8%', '12%'] },
            { name: 'Prob. Golpes críticos', values: ['1%', '3%', '5%', '10%'] },
            { name: 'Prob. Golpes perforadores', values: ['1%', '3%', '5%', '10%'] },
            { name: 'Prob. desmayo', values: ['1%', '5%', '8%'] },
            { name: 'Prob. retardo', values: ['1%', '5%', '8%'] },
            { name: 'Prob. sangrado', values: ['1%', '5%', '8%'] },
            { name: 'Prob. envenenamiento', values: ['1%', '5%', '8%'] }
        ];

        // Función para obtener un número aleatorio dentro de un rango
        function getRandomNumber(min, max) {
            return Math.floor(Math.random() * (max - min + 1)) + min;
        }

        // Función para generar nombres y porcentajes aleatorios
        function showRandomAttributes() {
            const overlay = document.getElementById('overlay');
            overlay.innerHTML = '';

            // Generar valores para Daño de media y Daño de habilidad
            let damageToMedia = getRandomNumber(-59, 59);
            let damageToAbility = getRandomNumber(-25, 25);

            // Asegurar que uno sea positivo y otro negativo
            if (damageToMedia >= 0 && damageToAbility >= 0) {
                damageToAbility = -damageToAbility; // Daño de habilidad negativo
            } else if (damageToMedia <= 0 && damageToAbility <= 0) {
                damageToMedia = -damageToMedia; // Daño de media positivo
            }

            const damageToMediaItem = document.createElement('div');
            damageToMediaItem.classList.add('overlay-item');
            damageToMediaItem.textContent = `Daño de media ${damageToMedia > 0 ? '+' : ''}${damageToMedia}%`;
            overlay.appendChild(damageToMediaItem);

            const damageToAbilityItem = document.createElement('div');
            damageToAbilityItem.classList.add('overlay-item');
            damageToAbilityItem.textContent = `Daño de habilidad ${damageToAbility > 0 ? '+' : ''}${damageToAbility}%`;
            overlay.appendChild(damageToAbilityItem);

            // Elegir tres atributos aleatorios
            const selectedAttributes = [];
            while (selectedAttributes.length < 3) {
                const randomAttribute = getRandomElement(attributes);
                if (!selectedAttributes.includes(randomAttribute) && randomAttribute.name !== 'Daño de media' && randomAttribute.name !== 'Daño de habilidad') {
                    selectedAttributes.push(randomAttribute);
                }
            }

            // Mostrar los atributos con porcentajes aleatorios
            selectedAttributes.forEach(attribute => {
                const randomValue = getRandomElement(attribute.values);
                const item = document.createElement('div');
                item.classList.add('overlay-item');
                item.textContent = `${attribute.name} ${randomValue}`;
                overlay.appendChild(item);
            });

            // Mostrar el widget
            document.getElementById('widget').classList.remove('hidden');

            // Ocultar el widget después de un tiempo definido
            setTimeout(function() {
                document.getElementById('widget').classList.add('hidden');
            }, 10000); // Cambia el tiempo según sea necesario
        }

        // Función para obtener un elemento aleatorio de una lista
        function getRandomElement(list) {
            return list[Math.floor(Math.random() * list.length)];
        }

        // Botones de prueba
        document.addEventListener('DOMContentLoaded', function() {
            const refreshButton = document.createElement('button');
            refreshButton.textContent = 'Refrescar';
            refreshButton.onclick = showRandomAttributes;
            document.body.appendChild(refreshButton);

            const emulateButton = document.createElement('button');
            emulateButton.textContent = 'testeo';
            emulateButton.onclick = showRandomAttributes;
            document.body.appendChild(emulateButton);
        });
    </script>
</body>
</html>

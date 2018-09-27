#### <span style="color:green">Clickexperts</span>
## Instructivo Formz
##### Guía de integración

###RESUMEN
Esta guía permite la integración de formularios **Formz** con Mailtrackpro para capturar sus resultados, mandar una copia por mail a un responsable y hacer el tracking de los visitantes desde la recepción del mail al final de su recorrido en landing.

###CONECCIÓN
URL: http://be.clickexperts.com/
User y Pass: preguntar a soporte

###INTEGRACIÓN DE FORMULARIO
1. **Creación del formulario:**
    * Ir a “New Form” y llenar los campos siguientes:
    **Title:** Título del formulario
    **To email:** Mail del responsable que recibe los resultados de los formularios
    **From email:** Mail del remitente
    **Subject:** Asunto del mail
    * Después de la validación, anotar el resultado del campo “id” para usar en el javascript

2. **Tracking**
    1. **Mail:**
    Insertar el codigo siguiente en el cuerpo del mail. PROJECT_ID es un ID que soporte le va a comunicar.
        ```
        <img src="https://api.keen.io/3.0/projects/PROJECT_ID/events/formz?api_key=WRITE_KEY&data=ENCODED_DATA"/>
        ```
    1. **Landing Pages:**
    Insertar el codigo siguiente en las paginas del landing, justo antes del tag \</title\>. Tiee que reemplazar FORM_ID por el valor del id obtenido en la etapa 1.  PROJECT_ID y WRITE_KEY seran comunicados por soporte.
    
        ```
        <script>
        (function(name,path,ctx){var latest,prev=name!=='Keen'&&window.Keen?window.Keen:false;ctx[name]=ctx[name]||{ready:function(fn){var h=document.getElementsByTagName('head')[0],s=document.createElement('script'),w=window,loaded;s.onload=s.onerror=s.onreadystatechange=function(){if((s.readyState&&!(/^c|loade/.test(s.readyState)))||loaded){return}s.onload=s.onreadystatechange=null;loaded=1;latest=w.Keen;if(prev){w.Keen=prev}else{try{delete w.Keen}catch(e){w.Keen=void 0}}ctx[name]=latest;ctx[name].ready(fn)};s.async=1;s.src=path;h.parentNode.insertBefore(s,h)}}
        })('KeenAsync','https://d26b395fwzu5fz.cloudfront.net/keen-tracking-1.4.2.min.js',this);

        KeenAsync.ready(function(){
        const client = new KeenAsync({
            projectId: PROJECT_ID,
            writeKey: WRITE_KEY
        });

        // Record an event
        client.recordEvent('formz', {
            formID: FORM_ID,
            step: STEP_NUMBER
        });
        });
        </script>
        ```
        Este codigo es asyncrono y entonces no puede lentificar la carga de la pagina
    1. **Form**
    Se necesitan postear los datos del formulario al endpoint del API de formz: "http://be.iteas.co/api/responses/storeAndSend"
        1. El formulario tiene que integrar un campo HIDDEN con nombre "_form_key" y como valor el ID del formulario colectado en el paso 1.
        Ejemplo:
            ```
            <input type="hidden" name="_form_key" value="5afd811d4e912343e955732c7">
            ```
        1. Para el posteo, ejemplo de codigo javascript usando jQuery y el plugin "serializeObject":
        
            ```
            $.validate({
                form : '#contactForm',
                onSuccess : function($form) {
                    var url = 'http://be.iteas.co/api/responses/storeAndSend'
                    var jqxhr = $.ajax({
                        url: url,
                        method: "POST",
                        dataType: "json",
                        data:  {parameters: $form.serializeObject()}
                    }).success(
                        // do something like show a success page
                    );
                    return false;
                    }
                });
            ```
            contactForm siendo el ID del formulario en HTML


        1. Contactar soporte para validación de la integración.
   

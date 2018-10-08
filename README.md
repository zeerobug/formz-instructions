#### <span style="color:green">Clickexperts</span>
## Instructivo Formz
##### Guía de integración

### RESUMEN
Esta guía permite la integración de formularios **Formz** con Mailtrackpro para capturar sus resultados, mandar una copia por mail a un responsable y hacer el tracking de los visitantes desde la recepción del mail al final de su recorrido en landing.

### CONECCIÓN
URL: http://be.clickexperts.com/
User y Pass: preguntar a soporte

### INTEGRACIÓN DE FORMULARIO
1. **Creación del formulario:**  
    * Ir a “New Form” y llenar los campos siguientes:
    
    **Title:** Título del formulario
    
    **To email:** Mail del responsable que recibe los resultados de los formularios
    
    **From email:** Mail del remitente
    
    **Subject:** Asunto del mail
    
    **Tracking project ID** Para el seguimiento del mail, landing y form, entrar el ID comunicado por soporte 
    
    * Después de la validación, ir a la solapa "TAGS" para integrar los tags de seguimiento 

2. **Integración de los tags**
    1. **Mail:**
    Insertar el codigo del campo "Email Tag" en el cuerpo del mail.  
    por ejemplo:  
        ```
        <img src="https://api.keen.io/3.0/projects/PROJECT_ID/events/formz?api_key=WRITE_KEY&data=ENCODED_DATA"/>
        ```
    1. **Landing Pages:**  
    Insertar el codigo del campo "Landing Tag" en el HTML de la pagina landing, justo antes del tag \</title\>. Si el formulario tiene mas de una pagina, es necesario incrementar la variable step de 1 a cada pagina. En la ultima pagina (agradecimiento) se pone a "step" el valor "final".  
    por ejemplo:
        ```
        <script>
        (function(name,path,ctx){var latest,prev=name!=='Keen'&&window.Keen?window.Keen:false;ctx[name]=ctx[name]||{ready:function(fn){var h=document.getElementsByTagName('head')[0],s=document.createElement('script'),w=window,loaded;s.onload=s.onerror=s.onreadystatechange=function(){if((s.readyState&&!(/^c|loade/.test(s.readyState)))||loaded){return}s.onload=s.onreadystatechange=null;loaded=1;latest=w.Keen;if(prev){w.Keen=prev}else{try{delete w.Keen}catch(e){w.Keen=void 0}}ctx[name]=latest;ctx[name].ready(fn)};s.async=1;s.src=path;h.parentNode.insertBefore(s,h)}}
        })('KeenAsync','https://d26b395fwzu5fz.cloudfront.net/keen-tracking-1.4.2.min.js',this);

        KeenAsync.ready(function(){
        const client = new KeenAsync({
            projectId: "PROJECT_ID",
            writeKey: "WRITE_KEY"
        });

        // Record an event
        client.recordEvent('formz', {
            formID: "FORM_ID",
            step: "final"
        });
        });
        </script>
        ```
        Este codigo es asyncrono y entonces no puede lentificar la carga de la pagina  

    2. **Form**  
    Se necesitan postear los datos del formulario al endpoint del API de formz. Para esto, pegar en la pagina del formulario el codigo del campo "Formz API data".  Se tiene que reemplazar "formHtmlId" por el ID del formulario. Por ejemplo:

        ```
        serializeObject=function(form){"use strict";var a={},b=function(b,c){var d=a[c.name];"undefined"!=typeof d&&d!==null?$.isArray(d)?d.push(c.value):a[c.name]=[d,c.value]:a[c.name]=c.value};return $.each(form.serializeArray(),b),a};

        var formSelector = '#formHtmlId'
        $(formSelector).submit(
            function(e) {
                e.preventDefault()
                var form = $( formSelector )
                var url = 'http://api.clickexperts.com/api/responses/storeAndSend';
                $(this).append('<input type="hidden" name="_form_key" value="FORM_ID" /> ');
                $.post( url, {parameters: serializeObject(form)});
            }
        )
        ```

    1. Contactar soporte para validación de la integración.
   

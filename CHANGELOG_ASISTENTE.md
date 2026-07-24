# Fix: Asistente Virtual - Conexión y Manejo de Errores

## Problema
El asistente virtual mostraba el error genérico:
```
No se ha podido conectar. Inténtalo de nuevo o contacta con recepción.
```
Sin diferenciar entre:
- Timeout (webhook lento o no responde)
- Error de red
- Error HTTP

## Solución Aplicada

### 1. **Timeout de 8 segundos**
```javascript
var TIMEOUT=8000; // 8 segundos máximo
var controller=new AbortController();
var timeoutId=setTimeout(function(){controller.abort()},TIMEOUT);
```

### 2. **Mejor Manejo de Errores**
- ✅ Detecta `AbortError` → "Tardó demasiado"
- ✅ Otros errores → "No se ha podido conectar"
- ✅ Logging en consola para debugging

### 3. **Mensajes Claros al Usuario**
```javascript
if(err.name==='AbortError'){
  loading.textContent='Tardó demasiado. Inténtalo de nuevo o contacta con recepción.';
}else{
  loading.textContent='No se ha podido conectar. Inténtalo de nuevo o contacta con recepción.';
}
console.error('Error:',err.message);
```

## Archivos Modificados
- `index.html` - Script del asistente virtual

## Testing
1. Abre la web en Vercel
2. Haz clic en el botón de chat 💬
3. Prueba enviando un mensaje
4. Abre la consola (F12) para ver logs

## Próximos Pasos
- [ ] Verificar que el webhook de n8n responde correctamente
- [ ] Monitorear logs en Vercel
- [ ] Considerar agregar reintentos automáticos si falla

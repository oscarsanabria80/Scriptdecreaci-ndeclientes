# Scriptdecreaci-ndeclientes

<pre>
#!/bin/bash

# Asigna los argumentos a variables
NOMBRE_MAQUINA="$1"
TAMANO_VOLUMEN="$2"
NOMBRE_RED="$3"

# Ruta de la plantilla y ruta del nuevo volumen
PLANTILLA="/home/oscarsanabria/iso/plantilla-cliente-spa.qcow2"
NUEVO_VOLUMEN="/home/oscarsanabria/iso/$NOMBRE_MAQUINA.qcow2"

# Clona la máquina virtual y asigna el nuevo volumen
virt-clone --connect=qemu:///system --original plantilla-cliente --name "$NOMBRE_MAQUINA" --auto-clone --file "$NUEVO_VOLUMEN"

# Cambia el hostname de la nueva máquina
virt-customize --connect=qemu:///system -d "$NOMBRE_MAQUINA" --hostname "$NOMBRE_MAQUINA"

# Redimensiona el disco al tamaño especificado
qemu-img resize "$NUEVO_VOLUMEN" "$TAMANO_VOLUMEN"

# Inicia la nueva máquina virtual
virsh start "$NOMBRE_MAQUINA"

# Conecta la interfaz de red de la nueva máquina a la red especificada
virsh attach-interface --domain "$NOMBRE_MAQUINA" --type network --source "$NOMBRE_RED" --model virtio --config --live

echo "La máquina virtual $NOMBRE_MAQUINA ha sido creada y está en funcionamiento."
</pre>

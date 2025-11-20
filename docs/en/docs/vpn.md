1. Crear resource group en region deseada
2. Crear una virtual network en AZ, eligiendo su espacio de direcciones
3. Crear una subnet, con un subespacio de direcciones.
4. Crear otra subnet, de tipo Gateway, con otro subespacio, mas pequeño, pero no menor a /27. 
5. Crear virtual network gateway
    - Buscar en search, es la de marketplace.
    - Hay que usar la subnet de tipo Gateway

6. Crear VPC en AWS
    - Crear subnets
    - Crear route tables
7. Crear customer gateway en AWS utilizando la IP pública de la virtual network gateway previa.
8. Crear Virtual Private Gateway
    - Attach a la VPC creada previamente.
9. Crear Site-to-Site VPN
    - Tenes que asignarle la customer gateway de (7)
    - La Virtual Private Gateway de (8)
    - En static routes hay que poner la subnet de (3) (subnet de la VM)


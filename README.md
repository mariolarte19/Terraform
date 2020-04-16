# Skytap-DevOps-Terraform💻



## Requerimientos 

* Una maquina virtual ubuntu 
* Terraform v0.12.24
* Terraform-skytap provider v0.14.0
* Github para clonar el repositorio de código fuente.
* Acceso a cuenta Skytap.

## 1 Aprovisionamiento de infraestructura en Skytap desde una maquina local

### 1.1 Instale Terraform en su máquina local

a. Cree una carpeta en su sistema local que se llama terraform y navegue a su carpeta.

`mkdir terraform && cd terraform`

b. [Descargue la ùltima versión de Terraform CLI en su máquina local (en este caso es la 0.12.24)](https://releases.hashicorp.com/terraform/)

El archivo de instalación quedara alojado en la carpeta de descargas que tenga configurada por defecto, por lo que debe entrar a la c arpeta de descargas para extraer el archivo.

`cd $HOME/Downloads`

c. Extraiga el paquete Terraform y copie el archivo binario en su directorio terraform.

`unzip terraform_0.12.24_linux_amd64.zip`<br />
`sudo mv terraform $HOME/terraform`

d. Apunte la variable de entorno $ PATH a su archivo binario Terraform.

`export PATH=$PATH:$HOME/terraform`

e. Verifique que la instalación sea exitosa 

`terraform --version`

Vera una salida como la siguiente:

`Terraform v0.12.24`

### 1.2 Descargue el complemento skytap provider 

a. [Descargue la última versión del archivo binario de Skytap Provider.](https://releases.hashicorp.com/terraform-provider-skytap/)

Ingrese a la carpeta de descargas y extraiga el archivo binario del plug-in, para este caso se ha descargado la versión para Linux de 64 bits.

`cd $HOME/Downloads
unzip terraform-provider-skytap_0.14.0_linux_amd64.zip`

b. Cree una carpeta oculta para su complemento.

`mkdir $HOME/.terraform.d/plugins`

c. Mueva el complemento de Skytap Provider a la carpeta oculta que acaba de crear

`mv $HOME/Downloads/terraform-provider-ibm* $HOME/.terraform.d/plugins/`

d. Ingrese a la carpeta occulta y verifique que la instalación se haya terminado

`cd $HOME/.terraform.d/plugins && ./terraform-provider-ibm_*`

Vera una salida como la siguiente:

-
-
-

### 1.3 Configure el elemento Skytap provider

a. Cree una carpeta en su máquina local para su primer proyecto Terraform y navegue hacia la carpeta. Esta carpeta se utiliza para almacenar todos los archivos de configuración y definiciones de variables.

`cd $HOME`<br />
`mkdir myproject && cd myproject`

b. Cree un archivo de configuración config.tf, utilice este archivo para configurar el complemento de Skytap Provider con las credenciales

`nano config.tf`

El archivo tendrá la siguiente estructura

<pre><code>
provider "skytap" {
  username = "var.skytap_username"
  api_token = "var.skytap_api_token"
}


resource "skytap_environment" "enviroment"{
  template_id = "id"
  name = "Prueba"
  description = "Skytap terraform provider example environment."
}
</pre></code>

## 2. Aprovisionamiento de recursos en Skytap desde una IBM Schematics

### 2.1 Github

a. Crear un repositorio con los archivos terraforms vars.tf y main.tf.

* vars.tf contiene nuestras variables de autenticacion de Skytap. 

` variable "username" {
  description = "Enter your Skytap username"
}
variable "api_token" {
  description = "Enter your Skytap API token"
} `

* main.tf contiene el script de aprovisionamiento de recursos en Skytap.

` provider "skytap" {
  username = "${var.username}"
  api_token = "${var.api_token}"
}
resource "skytap_environment" "enviroment"{
  template_id = "id"
  name = "Prueba"
  description = "Skytap terraform provider example environment."
} ......`

### 2.1 Configuracion Schematics.

a. En IBM Schematics crear un espacio de trabajo.
b. Importar la plantilla de Terraform:

* Se copia la URL del repositorio de GitHub o GitLab el cual contiene la platilla de Terraform de aprovisunamiento de recursos en Skytap.
c. Recuperar Variables de entrada.

* Seleccionamos nuestras variables de entrada las cuales son User name y API key.
* Insertar variables de auteticacion las cuales se encuentran en nuestra cuenta de Skytap.
d. Creamos un espacio de trabajo 

e. Generar Plan.
Una vez creado el escpacio de trabajo generamos el plan de nuestra plantilla de Terraform atra vez del boton generar plan el cualsimula el comando de `terraform plan`  se usa para crear un plan de ejecución.


f. Aplicar plan
 Luego de generar el plan procedemos a aplicar nuestra plantilla de Terraform mediante el boton aplicar plan el cual simula el comando `terraform apply` , se usa para aplicar los cambios necesarios para alcanzar el estado deseado de la configuración.
 
 










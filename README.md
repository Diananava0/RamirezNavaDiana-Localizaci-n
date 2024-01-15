# RamirezNavaDiana-Localizaci-n
Objetivo. Gestionar los permisos de localización con una precisión definida y obtendremos la latitud y longitud donde se ubica el dispositivo.

Instalar la dependencia de localización de Google Play Services en el archivo build.gradle.kts ubicado en el apartado de Gradle Scripts, en el apartado de dependencias: implementación ("com.google.android.gms:play-services-location:21.0. 1")

Necesitamos pedir permiso para usar la localización, por lo que dentro del archivo AndroidManifest.xml, agregue las siguientes líneas dentro de la etiqueta:

<activity android:name=".MainActivity" tools:ignore="WrongManifestParent" android:exported="true">
    <intent-filter android:exported="true">
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
</activity>
En el Layout Principal, agregue las siguientes Vistas que serán dos vistas de texto que guardarán la latitud y longitud de la localización al presionar un botón:
En la clase Kotlin, configuramos las variables un id de permiso y el localizador de clientes, así como las variables de los Views: paquete com.example.gps

importar android.Manifest importar android.os.Bundle importar androidx.activity.ComponentActivity importar android.widget.TextView importar android.widget.Button importar com.google.android.gms.location.FusedLocationProviderClient importar androidx.core.app.ActivityCompat importar android .content.pm.PackageManager importar android.location.LocationManager importar android.content.Context importar android.annotation.SuppressLint importar com.google.android.gms.location.LocationServices

@SuppressLint("RestrictedApi") clase MainActivity: ComponentActivity(){

private lateinit var mFusedLocationClient: FusedLocationProviderClient privado lateinit var tvLatitude: TextView privado lateinit var tvLongitude: TextView privado lateinit var btnLocate: Botón

objeto complementario {const val PERMISSION_ID = 33}

anular diversión onCreate(savedInstanceState: ¿Paquete?) { super.onCreate(savedInstanceState) setContentView(R.layout.activity_main)

// Inicializar vistas después de inflar la interfaz de usuario
tvLatitude = findViewById(R.id.tvLatitude)
tvLongitude = findViewById(R.id.tvLongitude)
btnLocate = findViewById(R.id.btnLocate)

// Inicializar FusedLocationProviderClient
mFusedLocationClient = LocationServices.getFusedLocationProviderClient(this)

btnLocate.setOnClickListener {
    getLocation()
}
}

verificación de diversión privadaConcedido(permiso: Cadena): booleano { return ActivityCompat.checkSelfPermission(this, permiso) == PackageManager.PERMISSION_GRANTED }

diversión privada checkPermissions() = checkGranted(Manifest.permission.ACCESS_COARSE_LOCATION) && checkGranted(Manifest.permission.ACCESS_FINE_LOCATION)

requestPermissions divertidos privados() { ActivityCompat.requestPermissions(this, arrayOf(Manifest.permission.ACCESS_COARSE_LOCATION, Manifest.permission.ACCESS_FINE_LOCATION), PERMISSION_ID) }

diversión privada isLocationEnabled(): booleano { val locationManager: LocationManager = getSystemService(Context.LOCATION_SERVICE) as LocationManager return locationManager.isProviderEnabled(LocationManager.GPS_PROVIDER) || locationManager.isProviderEnabled(LocationManager.NETWORK_PROVIDER) }

@SuppressLint("MissingPermission") diversión privada getLocation() { if (checkPermissions()) { if (isLocationEnabled()) { mFusedLocationClient.lastLocation.addOnSuccessListener(this) { ubicación -> tvLatitude.text = ubicación?.latitude.toString( ) tvLongitude.text = ubicación?.longitude.toString() } } } else { requestPermissions() } } }

Creamos un método para consultar el estado de algún permiso en la aplicación. private fun checkGranted(permission: String): Boolean{ return ActivityCompat.checkSelfPermission(this, permiso) == PackageManager.PERMISSION_GRANTED } 6. Un código para comprobar si la aplicación tiene permisos de localización. diversión privada checkPermissions() = checkGranted(Manifest.permission.ACCESS_COARSE_LOCATION) && checkGranted(Manifest.permission.ACCESS_FINE_LOCATION)

Una función para pedir el permiso de localización por si aún no se lo otorgamos en la aplicación. requestPermissions divertidos privados() { ActivityCompat.requestPermissions( this, arrayOf(Manifest.permission.ACCESS_COARSE_LOCATION, Manifest.permission.ACCESS_FINE_LOCATION), PERMISSION_ID ) }

Consultamos si el GPS esta prendido. diversión privada isLocationEnabled(): booleano { var locationManager: LocationManager = getSystemService(Context.LOCATION_SERVICE) as LocationManager return locationManager.isProviderEnabled(LocationManager.GPS_PROVIDER) || locationManager.isProviderEnabled( LocationManager.NETWORK_PROVIDER )

}

Mostramos la localización en formato latitud y longitud en los TextViews correspondientes y asignamos el método getLocation al botón de localización. @SuppressLint("MissingPermission") diversión privada getLocation() { if (checkPermissions()) { if (isLocationEnabled()) { mFusedLocationClient.lastLocation.addOnSuccessListener(this) { ubicación -

tvLatitude.text = ubicación?.latitude.toString() tvLongitude.text = ubicación?.longitude.toString() } } } else{ requestPermissions() } }

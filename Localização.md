Codigo para verificar localicação e validar 

Set(LocalLatitude; "-3,0771663");;  // Latitude do local alvo
Set(LocalLongitude; "-59,9169417");; // Longitude do local al
Set(UserLatitude; Location.Latitude);;
Set(UserLongitude; Location.Longitude);;

Set(
    DistanciaMetros;
    6371000 * 
    2 * 
    Asin(
        Sqrt(
            Power(Sin((Radians(UserLatitude - LocalLatitude) / 2)); 2) +
            Cos(Radians(LocalLatitude)) * 
            Cos(Radians(UserLatitude)) * 
            Power(Sin((Radians(UserLongitude - LocalLongitude) / 2)); 2)
        )
    )
)

;;
If(
    DistanciaMetros <= 100;
    Notify("✅ Você está no local correto!"; NotificationType.Success);
    Notify("🚫 Você está fora da área permitida!"; NotificationType.Error)
)

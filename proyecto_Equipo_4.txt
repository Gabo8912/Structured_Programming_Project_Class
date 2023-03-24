#include<iostream>
#include<iomanip>
#include <ctime>

using namespace std;
int cont=5; // contador para saber cuantos productos tengo
string productos[100]={"leche","pan","agua","huevos","refresco"}; //Arreglo que guarda el Nombre del Producto
int id[100]={2,4,1,3,5}; // Arreglo para guardar el id
int existencias[100]={16,18,12,20,30}; // Arreglo para guardar las existencias
int reorden[100]={5,6,4,7,8}; // Arreglo para guardar el reorden
int st[100]={1,1,0,1,1}; // Arreglo para guardar el estatus
double pc[100]={12.35,5.5,13.39,22.4,10.99}; // Arreglo para guardar el precio Compra
double pv[100]={15.5,7.95,18.55,30.39,14.75};// Arreglo para guardar el precio Venta
int cont_u=3;//Contador para saber cuantos usuarios hay
string usuarios[100]={"admin","vend1","vend2"};//Arreglo para registrar usuarios
int tip_usuario[100]={1,2,2};//Areglo tipo de usuario
int st_usuario[100]={1,1,1}; //Arreglo estatus de usuarios
string password[100]={"123","123","123"};//Arreglo para contraseñas
string prod[100];//Arreglo dinamico que va a guardar temporalmente los productos vendidos 
int cant[100];//Arreglo dinamico que va a guardar temporalmente la Cantidad de productos vendidos 
double totalV[100];//Arreglo dinamico que va ir guardando los totales de los ticket
double totalVG[100];//Arreglo en el cual se van a guardar el corte de caja de cada Vendedor
int cont_tic;//Contador para saber cuantos productos llevo en el ticket 
int cont_clien=0;//Contador para saber cuantos clientes han realizado una Compra
int cont_ventG=0;//contador para saber el numero de corte de cajas de cada vendedor 
double egresos[100];//Arreglo dinamico para ir guardando los egresos de cada ticket
double egresG[100];//Arreglo que guarda el total de egresos de cada vendedor
string usg;//variable para saber que vendedor o usuario esta activo 



void admin();//Funcion que controla toda la area administrativa
void ventas();//Funcion que controla las ventas 
void menu_inventario();//Funcion que controla el menu de como mostrar los productos del inventario
void altas_inventario();//Funcion para dar de alta productos en el inventario
void consultas_inventario();//Funcion para hacer consultas en el inventario
void modificar_inventario();//Funcion para modificar productos del inventario
int buscar_prod(string produc_temp);//Funcion para buscar un producto
void ingreso_a_ventas();//Funcion que controla en ingreso a ventas
void bajas_inventario();//Funcion para dar de baja productos en el inventario
void admin_cuentas();//Funcion que controla el menu del Administrador de usuarios
void altas_usuario();//Funcion para dar de alta usuarios
void bajas_usuario();//Funcion para dar de baja un usuario
void consultas_usuario();//Funcion para consultar usuarios
void modificar_usuario();//Funcion para modificar usuarios
void mostrar_usuario();//Funcion para mostrar_usuarios
int buscar_usuario(string usua);//Funcion para buscar usuarios
void imprimirTicket();//Funcion para imprimir los tickets
void orden_por_producto();//Funcion para ordenar el inventario por Nombre
void orden_por_id();//Funcion para ordenar el inventario por id
void copias();//Funcion para copiar los Arreglos a la estructura
void corte_caja_ind();//Funcion para hacer el corte de caja por vendedor
void corte_caja_general();//Funcion para hacer el corte de caja general
void copiar_usuarios();

//estructura para guardar productos
struct elementos{
    int id;
    string producto;
    double pc;
    double pv;
    int existencias;
    int reorden;
    int st;
    
};
//estructura para guardar usuarios
struct usuario{
    string nom_usuario;
    int tipo_usuario;
    int stat_usuario;
    string passusuario;
};

struct elementos inventario[100];//Arreglo de estructura para guardar productos
struct usuario miusuario[100];//Arreglo de estructura para guardar usuarios

int main(){
    int op;
    do{
        cout<<setw(20)<<"Menu Principal"<<endl;
        cout<<"1.Administrador"<<endl;
        cout<<"2.Ventas"<<endl;
        cout<<"3.Salir"<<endl;
        cout<<"Opcion: ";
        cin>>op;
        switch (op){
            case 1: admin();
            break;
            case 2: ingreso_a_ventas();
            break;
            case 3: cout<<"Cerrando Sistema..."<<endl;
            break;
            default:cout<<"Opcion Invalida"<<endl;
            break;
            
        }
      } while (op!=3);
return 0;
}

//Funcion para controlar el area administrativa
void admin(){
    string nom, contrasena;
    int op;
    int pos;
        while (true){
            cout<<"Usuario: "; 
            cin>>nom;
            cout<<"Contraseña: "; 
            cin>>contrasena;
            pos=buscar_usuario(nom);
            if(tip_usuario[pos]==2){
                 cout<<"Acceso denegado"<<endl;
             }else{
        if (pos<0 ){ 
            cout<<"Usuario o contraseña incorrectos"<<endl;
        }
        else{
            if(contrasena==password[pos]){
                cout<<"Bienvenido "<<endl;
                usg=nom;
                break;
            }else{
                cout<<"Usuario o contraseña incorrectos"<<endl;
            }
            } 
            
        }
        }
    do{
        cout<<setw(20)<<"Menu Aministrador"<<endl;
        cout<<"1.Altas"<<endl;
        cout<<"2.Bajas"<<endl;
        cout<<"3.Consultas"<<endl;
        cout<<"4.Modificaciones"<<endl;
        cout<<"5.Mostrar inventario"<<endl;
        cout<<"6.Administracion de cuentas de usuarios"<<endl;
        cout<<"7.Corte de caja general"<<endl;
        cout<<"8.Regresar al menu anterior."<<endl;
        cout<<"Opcion: ";
        cin>>op;
        switch (op){
        case 1: altas_inventario();
            break;
        case 2: bajas_inventario();
            break;
        case 3:consultas_inventario();
            break;
        case 4: modificar_inventario();
            break;
        case 5: menu_inventario();
            break;
        case 6: admin_cuentas();
            break;
        case 7: corte_caja_general();
            break;
        case 8 : 
            break;
        default:cout<<"Valor invalido"<<endl;
            break;
        }
    } while (op!=8);
}
    
//Funcion para ingresar a ventas con usario y contraseña
void ingreso_a_ventas(){
    string nom,contrasena;
    int pos;
        
        while (true){
            cout<<"Usuario: "; 
            cin>>nom;
            cout<<"Contraseña: "; 
            cin>>contrasena;
             pos=buscar_usuario(nom);
             if(tip_usuario[pos]==1){
                 cout<<"Acceso denegado"<<endl;
             }else{
             if(pos<0){
                cout<<"Usuario o contraseña incorrectos"<<endl;
             }else{ 
                if(contrasena==password[pos]){
                cout<<"Bienvenido "<<endl;
                usg=nom;
                ventas();
                break;
                }else{
                cout<<"Usuario o contraseña incorrectos"<<endl;
             }
          }
                 
             }
             
       
            
        
}
}

//Funcion para controlar las ventas
void ventas(){
    string prod_temp;
    string resp;
    int i=0;
    int cant_temp;
    int busc;
        cout<<"Para finalizar el ticket use * "<<endl;
        cout<<"Para hacer el corte de caja use **"<<endl;
        while(true){
            cout<<"Producto: ";
            cin>>prod_temp;
            if(prod_temp=="*"){
                imprimirTicket();
                break;
            }else if(prod_temp=="**"){
                corte_caja_ind();
                break;
            }
            busc= buscar_prod(prod_temp);
                if(busc<0){
                cout<<"Producto no existente "<<endl;
               }  else if(existencias[busc]==0){
                  cout<<"No quedan existencias de este Producto"<<endl;   
                 }else{
                cout<<"Cantidad: ";
                cin>>cant_temp;
                if(cant_temp>=existencias[busc]){
                    cout<<"Solo quedan  "<<existencias[busc]<<endl;
                    cout<<"las quieres si/no: ";
                    cin>>resp;
                    if(resp=="si"){
                    cant_temp=existencias[busc];
                    existencias[busc]= existencias[busc]-cant_temp;
                    prod[i]=prod_temp;
                    cant[i]=cant_temp;
                    i++;
                    }else{
                        
                    }
                    
                }else{
                existencias[busc]=existencias[busc] -cant_temp;
                prod[i]=prod_temp;
                cant[i]=cant_temp;
                i++;
                }
            }
            cont_tic=i;
        }
}

//Funcion para imprimir el ticket
void imprimirTicket(){
    
    int pos;
    string nom_temp;
    string nom_temp2;
    double total;
    double totalEg;
    int j;
    double subeg[100];
    double subtotal[100];
    cout<<setw(55)<<"Abarrotera"<<endl;
    time_t tSac = time(NULL);      
    struct tm* tmP = localtime(&tSac);
    cout<<"\n"<<setw(75)<< "Hora: " << tmP->tm_hour << ":" << tmP->tm_min << ":"<< tmP->tm_sec << endl;
    cout<<setw(75)<< "Fecha: " << tmP->tm_mday << "/" << tmP->tm_mon+1 << "/"<< tmP->tm_year+1900 << endl;
    cout<<"\n Vendedor: "<<usg<<endl;
    cout<<endl;
    cout<<"-"<<setw(10)<<"Producto"<<setw(20)<<"Cantidad"<<setw(20)<<"Precio unitario"<<setw(20)<<"Subtotal"<<endl;
    for(int i=0;i<cont_tic;i++){
        cout<<"-"<<setw(10)<<prod[i]<<setw(20)<<cant[i]<<setw(20);
        nom_temp=prod[i];
        pos=buscar_prod(nom_temp);
        subtotal[i]=cant[i]*pv[pos];
        subeg[i]=cant[i]*pc[pos];
        cout<<pv[pos] <<setw(20)<<subtotal[i] <<endl;
        total+=subtotal[i];
        totalEg += subeg[i];
    }
    cout<<endl;
    cout<<setw(70)<<"Total= "<<total<<endl;;
    totalV[cont_clien]=total;
    egresos[cont_clien]=totalEg;
    cont_clien++;
    subeg[100];
    subtotal[100];
    prod[100];
    cant[100];
    total=0;
    totalEg=0;
    ventas();
}

//Funcion para el corte de caja por Vendedor
void corte_caja_ind(){
    int i=0;
    double subtotal;
    double subtotaleg;
    for(i=0;i<cont_clien;i++){
        subtotal+= totalV[i];
        subtotaleg+=egresos[i];
    }
    totalVG[cont_ventG]=subtotal;
    cout<<"Tu corte de caja es de: "<<totalVG[cont_ventG]<<endl;
    egresG[cont_ventG]=subtotaleg;
    cont_ventG++;
    totalV[100];
    egresos[100];
    prod[100];
    cant[100];
    cont_clien=0;
    cont_tic=0;
    
    
}

//Funcion para el corte de caja general
void corte_caja_general(){
    int i=0;
    double total;
    double totaleg;
    double utili;
    for(i=0;i<cont_ventG;i++){
        total+=totalVG[i];
        totaleg+=egresG[i];
    }
    utili= total - totaleg;
    cout<<"Ingresos: "<<total<<endl;
    cout<<"Egresos: "<<totaleg<<endl;
    cout<<"Utilidad: "<<utili<<endl;
    
    
}

//Funcion para Mostar el menu del inventario 
void menu_inventario(){
     int op;
     do{
        cout<<setw(20)<<"Mostar Inventario "<<endl;
        cout<<"Ordenado Por: "<<endl;
        cout<<"1.ID"<<endl;
        cout<<"2.Producto"<<endl;
        cout<<"3.Regresar al Menú Principal"<<endl;
        cout<<"Opcion: ";
        cin>>op;
        switch (op){
            case 1:orden_por_id();
            break;
            case 2: orden_por_producto();
            break;
            case 3:
            break;
            default:cout<<"Valor invalido"<<endl;
            break;
        }
     }while(op !=3);
}
// Funcion para altas en el inventario 
void altas_inventario(){
        int i= cont;
        int id_temp;
        string produc_temp;
        double pc_temp;
        double pv_temp;
        int Exis_Temp;
        int NR;
        int st_temp=1;
        int busc;
        string act;
        
    while (true){
        string op;
        cout << "Para Agregar producto presione cualquier tecla, Para salir presione * "<< endl;
        cin >>op;
        if(op =="*"){
            break;
        }
        cout << "Nombre del producto: ";
        cin >> produc_temp;
        busc= buscar_prod(produc_temp);
        if(busc==-2){
            cout<<"Este producto esta dado de baja"<<endl;
            cout<<"¿Deseas Activarlo de nuevo? si/no :  ";
            cin>>act;
             if(act=="si"){
                 for(int i=0;i<cont;i++){
                     if(produc_temp==productos[i]){
                         st[i]=1;
                     }
                 }
                 
             }else{
                 
             }
        }else if(busc>=0){
            cout<<"Este Producto ya existe";
            }else{
                productos[i]=produc_temp;
                cout<< "Precio Compra ";
                cin >> pc_temp;
                cout << "Precio venta: ";
                cin>> pv_temp;
                while(pv_temp < pc_temp){
                 cout<< "El precio de venta o el de compra es incorrecto\n";
                 cout<< "Precio Compra ";
                 cin >> pc_temp;
                 cout << "Precio venta: ";
                 cin>> pv_temp; 
            }
            cout << "Existencias: ";
            cin >> Exis_Temp;
            cout << "Numero Reorden: ";
            cin >> NR;
              while(Exis_Temp < NR ){
                 cout<< "Las Existencias o el numero de Reorden es incorrecto"<<endl;
                 cout << "Existencias: ";
                 cin >> Exis_Temp;
                 cout << "Numero Reorden: ";
                 cin >> NR;
            }
            id_temp=i +1;
            id[i]=id_temp;
            pc[i]=pc_temp;
            pv[i]=pv_temp;
            existencias[i]=Exis_Temp;
            reorden[i]=NR;
            st[i]=st_temp;
            i++;
        }
    cont=i;
}
}
//Funcion para hacer las consultas en el inventario
void consultas_inventario(){
     copias();
     string consulta;
      int i;
      
    while (true){
        cout<<"Si quieres Regresar al menu Principal presiona * "<<endl;
        cout<<"Ingresa el producto a consultar: ";
        cin>>consulta;
        if(consulta=="*"){
            break;
        }
          i=buscar_prod(consulta);
          
          if(i<0){
              cout<<"Producto no existente o dado de baja"<<endl;
             
          }else{
            cout<<"Id"<<setw(10)<<"Producto"<<setw(10)<<"PC"<<setw(10)<<"PV"<<setw(20)<<"Existencias";
            cout<<setw(20)<<"Nivel de Reorden"<<endl;
            cout<<inventario[i].id<<setw(10)<<inventario[i].producto<<setw(10)<<inventario[i].pc<<setw(10);
            cout<<inventario[i].pv<<setw(20)<<inventario[i].existencias<<setw(15)<<inventario[i].reorden<<endl;
            
        }
 
        }
        
    }
    
    

//Funcion para hacer modificaciones en el inventario
void modificar_inventario(){
    string nom_temp;
    int busc;
    int op;
    int Exist_temp,NR_temp;
    double pc_temp, pv_temp;
    
    do {
            
            cout<<"Si quieres Regresar al menu Principal presiona * "<<endl;
            cout<<"Ingresa el nombre del producto que quieres modificar "<<endl;
            cout<<"Producto: ";
            cin >> nom_temp;
            if(nom_temp=="*"){
                break;
            }
            busc=buscar_prod(nom_temp);
            if(busc<0){
                cout<<"Producto no existente o dado de baja"<<endl;
                
            }else{
            cout<< "1. Precio de Compra"<<endl;
            cout<< "2. Precio de Venta"<<endl;
            cout<< "3. Existencias "<<endl;
            cout<< "4. Número de Reorden"<<endl;
            cout<< "5. Regresar al Menú anterior"<<endl;
            cout<< "Opción: ";
            cin >> op;
            switch(op){
                case 1: cout<<"Ingresa el nuevo Precio de Compra: ";
                        cin>>pc_temp;
                        pc[busc]=pc_temp;
                break;
                case 2:cout<<"Ingresa el nuevo Precio de Venta: ";
                        cin>>pv_temp;
                        pv[busc]=pv_temp;
                break;
                case 3: cout<<"Ingresa el nuevo Número de Existencias: ";
                        cin>>Exist_temp;
                        existencias[busc]=Exist_temp;
                break;
                case 4: cout<<"Ingresa el nuevo Número de Reorden: ";
                        cin>>NR_temp;
                        reorden[busc]=NR_temp;
                break;
                case 5:
                break;
                default: cout<<"Valor invalido"<<endl;
                break;
            }
            }
        }while (op !=5);
    
    }

//Funcion para buscar productos
int buscar_prod(string produc_temp){
        int i=-1;
        int r;
        while(i<cont and produc_temp!=productos[i]){
            i++;
             if(i==cont){
                 r= -1;
             }else
                   if(st[i]==0){
                    r= -2;
                }else{
                 r= i; 
             }
        }
        return r;
    }
//Funcio para dar de baja productos    
void bajas_inventario(){
    string prod_temp;
    int pos;
    cout<<"Para salir presione * "<<endl;
    while(true){
    cout<< "Ingrese el nombre del producto que quiere dar de baja: ";
    cin>>prod_temp;
    if(prod_temp=="*"){
        break;
    }
    pos=buscar_prod(prod_temp);
    if(pos<0){
        cout<<"Producto no existente o dado de baja "<<endl;
      }   else{
          cout<<"Producto dado de baja exitosamente"<<endl;
         st[pos]=0;
       }
        
      }
        
    }
//Funcion para la Administracion de cuentas de usuario    
void admin_cuentas(){
        int op;
        do{
        cout<<setw(20)<<"Menu Aministrador de usuarios"<<endl;
        cout<<"1.Altas"<<endl;
        cout<<"2.Bajas"<<endl;
        cout<<"3.Consultas"<<endl;
        cout<<"4.Modificaciones"<<endl;
        cout<<"5.Mostrar cuentas de usuario"<<endl;
        cout<<"6.Regresar al Menu Principal"<<endl;
        cout<<"Opcion: ";
        cin>>op;
        switch (op){
        case 1: altas_usuario();
            break;
        case 2: bajas_usuario();
            break;
        case 3:consultas_usuario();
            break;
        case 4: modificar_usuario();
            break;
        case 5: mostrar_usuario();
            break;
        default: cout<<"Opcion invalida "<<endl; 
         break;
        }
      }while(op!=6);
    }
    
//Funcion para buscar usuarios
int buscar_usuario(string usua){
        int i=-1;
        int r;
        while(i<cont_u and usua!=usuarios[i]){
            i++;
             if(i==cont_u){
                 r= -1;
             }else
                   if(st_usuario[i]==0){
                    r= -2;
                }else{
                 r=i; 
             }
        }
        return r;
        
    }

//Funcion para dar de alta un usuario
void altas_usuario(){
        int k=cont_u;
        string nombre_temp;
        string acti;
        int tipo;
        int st_temp=1;
        int pos;
        string contra_temp;
        
      
        while(true){
             cout<<"Para salir presione * "<<endl;
             cout<<"Ingrese el nombre del usuario: ";
             cin>>nombre_temp;
             if(nombre_temp=="*"){
                 break;
             }
             pos=buscar_usuario(nombre_temp);
             if(pos==-2){
                 cout<<"Este Usuario esta dado de baja"<<endl;
                 cout<<"¿Quieres reactivarlo? si/no ";
                 cin>>acti;
                 if(acti=="si"){
                     for(int j=0;j<cont_u;j++){
                         if(nombre_temp==usuarios[j]){
                             cout<<"Usuario reactivado ";
                             st_usuario[j]=1;
                         }
                     }
                 }else{
                     
                 }
            }else if(pos>=0){
                    cout<<"Este Usuario ya existe"<<endl;
             }else{
                  usuarios[k]=nombre_temp;
                  cout<<"Ingrese la contraseña: ";
                  cin>>contra_temp;
                  cout<<"Ingrese el tipo de usuario: ";
                  cin>>tipo;
                   while(true){
                      if(tipo>2){
                          cout<<"Tipo de usuario invalido "<<endl;
                          cout<<"Ingrese el tipo de usuario: ";
                          cin>>tipo;
                      }else{
                          break;
                      }
                  }
                  
                  password[k]=contra_temp;
                  tip_usuario[k]=tipo;
                  st_usuario[k]=st_temp;
                  k++;
              }
            cont_u=k;
        }
     
        
    }
    
//Funcion para dar de baja un usuario    
void bajas_usuario(){
    string nom_temp;
    int pos;
    cout<<"Para salir presione * "<<endl;
    while(true){
    cout<< "Ingrese el nombre del usuario que quiere dar de baja: ";
    cin>>nom_temp;
    if(nom_temp=="*"){
        break;
    }
    pos=buscar_usuario(nom_temp);
    if(pos<0){
        cout<<"usuario no existente o dado de baja "<<endl;
      }   else{
          cout<<"usuario dado de baja exitosamente "<<endl;
          st_usuario[pos]=0;
       }
        
      }
        
    }
    
//Funcion para hacer consultas de usuario
void consultas_usuario(){
     copiar_usuarios();
     string consulta;
     int i;
      cout<<"Si quieres Regresar al menu Principal presiona * "<<endl;
    while (true){
        cout<<"Ingresa el usuario a consultar: ";
        cin>>consulta;
        if(consulta=="*"){
            break;
        }
          i=buscar_usuario(consulta);
          
          if(i<0){
              cout<<"usuario no existente o dado de baja"<<endl;
             
          }else{
            cout<<"usuario"<<setw(15)<<"contraseña"<<setw(15)<<"Tipo"<<endl;
            cout<<miusuario[i].nom_usuario<<setw(15)<<miusuario[i].passusuario<<setw(15)<<miusuario[i].tipo_usuario<<endl;
        }
 
        }
        
    }
    
//Funcion para mostrar los usuario    
void mostrar_usuario(){
        copiar_usuarios();
        int i;
            cout<<"usuario"<<setw(15)<<"contraseña"<<setw(15)<<"Tipo"<<endl;
            for(i=0;i<cont_u;i++){
                if(miusuario[i].stat_usuario==0){
                    
                }else{
                cout<<usuarios[i]<<setw(15)<<password[i]<<setw(15)<<tip_usuario[i]<<endl;
                }
                
            }
    }

//Funcion para modificar usuarios    
void modificar_usuario(){
    string nom_us;
    int busc;
    int op;
    string contra;
    int tip;
    
    do {
            
            cout<<"Si quieres Regresar al menu Principal presiona * "<<endl;
            cout<<"Ingresa el nombre del usuario que quieres modificar "<<endl;
            cout<<"Usuario: ";
            cin >> nom_us;
            if(nom_us=="*"){
                break;
            }
            busc=buscar_usuario(nom_us);
            if(busc<0){
                cout<<"usuario no existente o dado de baja"<<endl;
                
            }else{
            cout<< "1.contraseña"<<endl;
            cout<< "2.Tipo de usuario"<<endl;
            cout<< "3.Regresar al Menú anterior "<<endl;
            cout<< "Opción: ";
            cin >> op;
            switch(op){
                case 1: cout<<"Ingresa la nueva contraseña: ";
                        cin>>contra;
                        password[busc]=contra;
                break;
                case 2:cout<<"Ingresa el nuevo Tipo de usuario: ";
                        cin>>tip;
                        tip_usuario[busc]=tip;
                break;
                default: cout<<"Valor invalido"<<endl;
                break;
            }
            }
        }while (op !=3);
    
    }
    
//Funcion para copiar los productos de los Arreglos a las estructuras
void copias(){
    int i;
    for(i=0;i!=cont;i++){
        inventario[i].id=id[i];
        inventario[i].producto=productos[i];
        inventario[i].pc=pc[i];
        inventario[i].pv=pv[i];
        inventario[i].st=st[i];
        inventario[i].existencias=existencias[i];
        inventario[i].reorden=reorden[i];
    }
}

//Funcion para Mostrar el inventario por id usando las estructuras
void orden_por_id(){
    copias();
    int i,j;
    struct elementos aux[100];

    for(i=0;i<cont;i++){
        for(j=0;j<(cont-1)-i;j++)
            if(inventario[j].id > inventario[j+1].id){
                aux[j]=inventario[j];
                inventario[j]=inventario[j+1];
                inventario[j+1]=aux[j];
            }
    }
    cout<<endl;
    cout<<"ID"<<setw(15)<<"PRODUCTO"<<setw(15)<<"PC"<<setw(15)<<"PV"<<setw(15)<<"EXISTENCIAS"<<setw(15)<<"REORDEN"<<setw(15)<<"RESURTIR"<<endl;
    for ( i=0;i<cont;i++){
        if(inventario[i].st==0){
            
        }else{
        cout<<inventario[i].id<<setw(15)<<inventario[i].producto<<setw(15)<<inventario[i].pc;
        cout<<setw(15)<<inventario[i].pv<<setw(15)<<inventario[i].existencias<<setw(15);
        cout<<inventario[i].reorden;  
        
        if(inventario[i].existencias<=inventario[i].reorden){
            cout<<setw(15)<<"*";
        }
        cout<<endl;
        }
    } 
}
//Funcionpara mostrar inventario ordenado por productos usando las estructuras
void orden_por_producto(){
    copias();
    int i,j;
    struct elementos aux[100];

    for(i=0;i<cont;i++){
        for(j=0;j<(cont-1)-i;j++)
            if(inventario[j].producto>inventario[j+1].producto){
                aux[j]=inventario[j];
                inventario[j]=inventario[j+1];
                inventario[j+1]=aux[j];
            }
    }
    cout<<endl;
    cout<<"ID"<<setw(15)<<"PRODUCTO"<<setw(15)<<"PC"<<setw(15)<<"PV"<<setw(15)<<"EXISTENCIAS"<<setw(15)<<"REORDEN"<<setw(15)<<"RESURTIR"<<endl;
    for ( i=0;i<cont;i++){
        if(inventario[i].st==0){
            
        }else{
        cout<<inventario[i].id<<setw(15)<<inventario[i].producto<<setw(15)<<inventario[i].pc;
        cout<<setw(15)<<inventario[i].pv<<setw(15)<<inventario[i].existencias<<setw(15);
        cout<<inventario[i].reorden;  
        if(inventario[i].existencias<=inventario[i].reorden){
            cout<<setw(15)<<"*";
        }
        cout<<endl;    
        }
      
    } 
}

//Funcion para copiar los usuarios de los Arreglos a la estructura
void copiar_usuarios(){
    int i;
    for(i=0;i<cont_u;i++){
        miusuario[i].nom_usuario=usuarios[i];
        miusuario[i].tipo_usuario=tip_usuario[i];
        miusuario[i].stat_usuario=st_usuario[i];
        miusuario[i].passusuario=password[i];
    }
}


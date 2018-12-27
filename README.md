# Class DataBase - Python&MySQL

Se ha desarrollado una clase que pretende facilitar la conexion a una base de dato en MySQL y además, facilitar la ejecución de distintas querys en MySQL desde python como crear tablas, insertar valores, etc.

Se ha usado la libreria pymysql

Aún está en un estado bastante pobre como para utilizarse a la hora de automatizar y simplificar las distintas operaciones en una base de datos en MySQL, pero los resultados que dan son buenos para hacer lo básico.

```python
import pymysql as pms
# from Curso.database.excepciones import rafaException      # -- Exception
class DataBase(object):
    def __init__(self,host,puerto,usuario,password,database):
        self.conexiondb=None
        self.host = host
        self.puerto= puerto
        self.usuario = usuario
        self.passwd=password
        self.database=database

    def openDB(self):
        self.conexiondb = pms.connect(host=self.host,
                                      port=self.puerto,
                                      user=self.usuario,
                                      passwd=self.passwd,
                                      db=self.database) # Tambien podemos añadir la opcion autocommit=True, de esta forma no tendriamos que cerrar las querys
    def closeDB(self):
        self.conexiondb.close()
    def __executeSQL(self,sql):
        cursor = self.conexiondb.cursor()
        cursor.execute(sql)
        self.conexiondb.commit() # Es importante poner el commit para cada query, sino no hace nada
    def ejecutaSQL(self,sql):
        self.__executeSQL(sql)
    def crearTabla(self,nombre, *args):
        nombre = str(nombre)
        sql = "create table {nom}(Id{nom} int not null primary key, {nom} varchar(50) not null)".format(nom=nombre)
        self.__executeSQL(sql)
        for i in args:
            sql = "alter table {} add {} varchar(500) not null".format(nombre, str(i))
            self.__executeSQL(sql)
        sql = "alter table {nom} drop column {nom}".format(nom=nombre)
        self.__executeSQL(sql)
    def insert(self,tabla,*args):
        tabla = str(tabla)
        values = "values('"
        values += str(args[0])
        for i in range(1,len(args)):
            values +="',"+ "'" + str(args[i])
        values += "')"
        sql = "insert into {} {}".format(tabla,values)
        self.__executeSQL(sql)

```


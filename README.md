**jango.jobcenter demo**

1. import Jango.JobCenter
 <PackageReference Include="Jango.JobCenter" Version="1.0.0" />
 or dotnet add package Jango.JobCenter --version 1.0.0

2. edit Starup.cs
   add using Jango.JobCenter;
   
 
            services.AddControllers();

            // add jango jobcenter
            // use memorystorage by  default
            services.AddJangoJobCenter();

            //or use Mongo /  sqlserver / mysql ...
            //services.AddJangoJobCenter(x => x.UseMongoStorage("...."));
        
       
3. add  app.UseJangoJobCenter()  before UseEndpoints;
   
    


            /// use jango jobcenter
            app.UseJangoJobCenter();

            app.UseEndpoints(endpoints =>
            {
                endpoints.MapControllers();
            });

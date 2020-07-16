#Jango.JobCenter.demo
jango.jobcenter demo

1. import Jango.JobCenter
 <PackageReference Include="Jango.JobCenter" Version="1.0.0" />
 or dotnet add package Jango.JobCenter --version 1.0.0

2. edit Starup.cs
   add using Jango.JobCenter;
   
   public void ConfigureServices(IServiceCollection services)
        {
            services.AddControllers();

            // add jango jobcenter
            // use memorystorage by  default
            services.AddJangoJobCenter();

            //or use Mongo /  sqlserver / mysql ...
            //services.AddJangoJobCenter(x => x.UseMongoStorage("...."));
        
        }
3. add  app.UseJangoJobCenter()  before UseEndpoints;
   
     public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
        {
            if (env.IsDevelopment())
            {
                app.UseDeveloperExceptionPage();
            }

            app.UseHttpsRedirection();

            app.UseRouting();
            app.UseAuthorization();

            /// use jango jobcenter
            app.UseJangoJobCenter();

            app.UseEndpoints(endpoints =>
            {
                endpoints.MapControllers();
            });
        }

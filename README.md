using Hangfire;
using PagedList;
using Q10.Core;
using Q10.Core.Logging;
using Q10.Jack.Areas.API.v1.EstructuraAcademica.Models.Cursos;
using Q10.Jack.Areas.Comunidad.ViewModels.Documentos;
using Q10.Jack.Areas.API.v1.EstructuraAcademica.Models.Cursos;
using Q10.Jack.Areas.Comunidad.ViewModels.Personas;
using Q10.Jack.Areas.Comunidad.ViewModels.Suggestions;
using Q10.Jack.Areas.Establecimiento.ViewModels.Encuestas;
using Q10.Jack.Areas.Establecimiento.ViewModels.Periodos;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.Actividades;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.Agrupaciones;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.Areas;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.Asignaturas;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.Asignaturas.AsignaturaCompetencia;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.AsignaturasEquivalentes;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.CapacidadesTerminales;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.CategoriasCancelacion;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.CausasCancelacion;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.Competencias;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.Correquisitos;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.CriteriosEntrevista;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.CriteriosEvaluacion;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.CruceHorario;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.DescuentosInstitucionalesCafam;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.DocentesApoyo;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.DocentesReemplazo;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.Empresas;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.Empresas.Personalizaciones;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.EscalaEvaluacion;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.EscalaNacional;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.EscalaValorativa;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.Especialidades;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.Evidencias;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.Grados;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.GradosSedeJornada;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.Grupos;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.GruposTrabajo;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.Horarios;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.IndicadoresLogro;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.InstitucionesEducativas;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.Modulos;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.OfertasAcademicas;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.ParametrosEvaluacionPracticas;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.PeriodosGrado;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.Personalizaciones;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.Personalizaciones.AdministrativoAsignatura;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.Personalizaciones.ComfaNorte;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.Personalizaciones.CupoProgramaZonaCinco;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.Personalizaciones.FacultadesAmazonia;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.Personalizaciones.Interactuar;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.Personalizaciones.LeccionesAsignaturas;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.PlanesAcademicos;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.Prerrequisitos;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.Programas;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.ProgramasSedeJornada;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.RequisitosGrado;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.RequisitosMatricula;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.RequisitosVuelo;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.TemasAsignatura;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.TiposCancelacion;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.TiposContratos;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.TiposIngresos;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.TiposProgramas;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.TiposSeguimientos;
using Q10.Jack.Areas.EstructuraAcademica.ViewModels.Unidades;
using Q10.Jack.Areas.GestionAcademica.Services;
using Q10.Jack.Areas.GestionAcademica.ViewModels.PracticasLaborales;
using Q10.Jack.Areas.Reportes.ViewModels;
using Q10.Jack.Data.Servicios.Suggestions;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;

namespace Q10.Jack.Data.Servicios
{
    public class ServicioEstructuraAcademica : BaseServicio
    {
        public const string CategoriaAuditoria = "05";

        #region Asignaturas

        public IList<AsignaturaViewModel> ObtenerAsignaturasParaAsignarPracticas(int? matriculaPrograma)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "matpro_consecutivoP", matriculaPrograma }
            };

            return institucion.BaseDatos.ExecuteQuery<AsignaturaViewModel>("pa_aca_asignaturas_para_asignar_practicas_laboral_retornar", parametros);
        }

        public IList<AsignaturaViewModel> ObtenerAsignaturasIndividualesEvaluaciones()
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "rol_codigoP", rol_codigoP },
                { "per_codigoP", per_codigoP }
            };

            return institucion.BaseDatos.ExecuteQuery<AsignaturaViewModel>("pa_eva_asignaturas_individuales_retornar", parametros);
        }

        public IList<AsignaturaViewModel> ObtenerAsignaturas(bool? estado, string texto, char? tipoEvaluacion = null, int? grado = null, int? periodo = null, bool? requiereEvaluacion = null, char? filtro = null)
        {

            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_estado", estado },
                { "filtros_texto", texto },
                { "gra_consecutivoP", grado },
                { "per_consecutivoP", periodo },
                { "per_codigoP", per_codigoP },
                { "rol_codigoP", rol_codigoP },
                { "asi_tipo_evaluacion", tipoEvaluacion },// I: Individual, G: Grupal 
                { "asi_requiere_evaluacion", requiereEvaluacion }, // Filtro para solo retornar en CAFAM las asignaturas que no sean de convicencia
                { "filtro_tipo", filtro }
            };

            return institucion.BaseDatos.ExecuteQuery<AsignaturaViewModel>("pa_aca_asignaturas_retornar", parametros);
        }

        public async Task<IList<AsignaturaViewModel>> ObtenerAsignaturasAsync(bool? estado, string texto, char? tipoEvaluacion = null, int? grado = null, int? periodo = null, bool? requiereEvaluacion = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_estado", estado },
                { "filtros_texto", texto },
                { "gra_consecutivoP", grado },
                { "per_consecutivoP", periodo },
                { "per_codigoP", per_codigoP },
                { "rol_codigoP", rol_codigoP },
                { "asi_tipo_evaluacion", tipoEvaluacion },// I: Individual, G: Grupal 
                { "asi_requiere_evaluacion", requiereEvaluacion } // Filtro para solo retornar en CAFAM las asignaturas que no sean de convicencia
            };

            return await institucion.BaseDatos.ExecuteQueryAsync<AsignaturaViewModel>("pa_aca_asignaturas_retornar", parametros);
        }

        public IList<AsignaturaViewModel> ObtenerAsignaturasArchivadas(bool? estado, string texto, char? tipoEvaluacion = null, int? sedeJornada = null, int? periodo = null, string programa = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_estado", estado },
                { "filtros_texto", texto },
                { "per_consecutivoP", periodo },
                { "sedjor_consecutivoP", sedeJornada },
                { "pro_codigoP", programa },
                { "asi_tipo_evaluacion", tipoEvaluacion }// I: Individual, G: Grupal, C: Cursos 
            };

            return institucion.BaseDatos.ExecuteQuery<AsignaturaViewModel>("pa_aca_asignaturas_archivadas_retornar", parametros);
        }

        public AsignaturaViewModel ObtenerDetalleAsignatura(string codigo, string codigoOpt = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "asi_codigoP", codigo },
                { "opt_codigoP", codigoOpt }
            };

            return institucion.BaseDatos.ExecuteQuery<AsignaturaViewModel>("pa_aca_asignaturas_detalle_retornar", parametros).FirstOrDefault();
        }

        public async Task<AsignaturaViewModel> ObtenerDetalleAsignaturaAsync(string codigo, string codigoOpt = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "asi_codigoP", codigo },
                { "opt_codigoP", codigoOpt }
            };

            return (await institucion.BaseDatos.ExecuteQueryAsync<AsignaturaViewModel>("pa_aca_asignaturas_detalle_retornar", parametros)).FirstOrDefault();
        }

        public async Task<AsignaturaViewModel> ObtenerDetalleAsignaturaAsync(string codigo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "asi_codigoP", codigo }
            };

            return (await institucion.BaseDatos.ExecuteQueryAsync<AsignaturaViewModel>("pa_aca_asignaturas_detalle_retornar", parametros)).FirstOrDefault();
        }

        #region Apis asignaturas

        public async Task<IPagedList<AsignaturaViewModel>> ObtenerAsignaturasAreaAsync(bool? estado, int pagina = 1, int tamanioPagina = Constantes.PAGINACION_TAMANO_PAGINA)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_estado", estado },
            };

            var result = await institucion.BaseDatos.ExecuteQueryAsync<AsignaturaViewModel>("pa_apis_aca_areas_asignaturas_estado_retornar", parametros, pagina, tamanioPagina);
            return new StaticPagedList<AsignaturaViewModel>(result.Item1, pagina, tamanioPagina, result.Item2);
        }

        public void IngresarAsignaturaApi(string codigoArea, string codigoAsig, string nombreAsig, string abreviacionAsig, bool? aplicaPromocion, bool? requiereEval, bool? estado)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "asi_are_codigo", codigoArea },
                { "asi_codigoP", codigoAsig },
                { "asi_nombre", nombreAsig },
                { "asi_abreviacion", abreviacionAsig },
                { "asi_promocion", aplicaPromocion },
                { "asi_requiere_evaluacion", requiereEval },
                { "asi_estado", estado },
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_apis_aca_asignaturas_ingresar", parametros);
        }

        public async Task<AsignaturaViewModel> ObtenerDetalleAsignaturaAreaAsync(string codigo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "asi_codigo", codigo }
            };

            return (await institucion.BaseDatos.ExecuteQueryAsync<AsignaturaViewModel>("pa_apis_aca_asignaturas_areas_retornar", parametros)).FirstOrDefault();
        }

        #endregion

        public void IngresarAsignatura(string codigo, string nombre, string abreviacion, bool estado, char? tipo, bool practica, bool aplicaInformes, char? tipo_evaluacion = null, string tipoPersonalizado = null,
                                       bool? seminarioGratuito = false, char? modalidad = null, char? tipoIndicadorLogro = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "asi_codigoP", codigo },
                { "asi_nombre", nombre },
                { "asi_abreviacion", abreviacion },
                { "asi_estado", estado },
                { "asi_tipo", tipo },
                { "asi_practicas", practica },
                { "asi_aplica_informes", aplicaInformes },
                { "asi_tipo_evaluacion", tipo_evaluacion },
                { "asi_variable1", tipoPersonalizado },
                { "asi_seminario_gratuito", seminarioGratuito },
                { "asi_modalidad", modalidad },
                { "asi_aplica_promedio_indicadores", tipoIndicadorLogro },
                { "aud_usuario_sesion", per_codigoP },
                { "aud_ip",ClientIpAddress },
                { "aud_cat_codigo", CategoriaAuditoria },
                { "aud_usuario_q10", codigo_ejecutivo_institucion },
                { "aud_interno", false }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_asignaturas_ingresar", parametros);
        }

        public void ModificarAsignatura(string codigo, string nombre, string abreviacion, bool estado, char? tipo, bool practica, bool aplicaInformes, char? tipo_evaluacion = null, string tipoPersonalizado = null,
                                        bool? seminarioGratuito = false, char? modalidad = null, char? tipoIndicadorLogro = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "asi_codigoP", codigo },
                { "asi_nombre", nombre },
                { "asi_abreviacion", abreviacion },
                { "asi_estado", estado },
                { "asi_tipo", tipo },
                { "asi_practicas", practica },
                { "asi_aplica_informes", aplicaInformes },
                { "asi_tipo_evaluacion", tipo_evaluacion },
                { "asi_variable1", tipoPersonalizado },
                { "asi_seminario_gratuito", seminarioGratuito },
                { "asi_modalidad", modalidad },
                { "asi_aplica_promedio_indicadores", tipoIndicadorLogro }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_asignaturas_modificar", parametros);
        }

        public void EliminarAsignatura(string codigo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "asi_codigoP", codigo }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_asignaturas_eliminar", parametros);
        }

        #endregion

        #region Asignaturas Area

        public IList<AsignaturaViewModel> ObtenerAsignaturasArea(string codigoArea, bool? estado = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "are_codigoP", codigoArea },
                { "filtros_estado", estado }
            };

            return institucion.BaseDatos.ExecuteQuery<AsignaturaViewModel>("pa_aca_asignaturas_colegios_retornar", parametros);
        }

        public AsignaturaViewModel ObtenerDetalleAsignaturaArea(string codigo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "asi_codigoP", codigo }
            };

            return institucion.BaseDatos.ExecuteQuery<AsignaturaViewModel>("pa_aca_asignaturas_colegios_detalle_retornar", parametros).FirstOrDefault();
        }

        public void IngresarAsignaturaArea(string codigoAsignatura, string nombre, string abreviacion, string codigoArea, bool promocion, bool estado, bool requiereEvaluacion, bool aplicaAutomatizacion = false)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "asi_codigoP", codigoAsignatura },
                { "asi_nombre", nombre },
                { "asi_abreviacion", abreviacion },
                { "asi_are_codigo", codigoArea },
                { "asi_promocion", promocion },
                { "asi_requiere_evaluacion",requiereEvaluacion },
                { "asi_estado", estado },
                { "asi_aplica_automatizacion", aplicaAutomatizacion }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_asignaturas_colegios_ingresar", parametros);
        }

        public void ModificarAsignaturaArea(string codigo, string nombre, string abreviacion, bool promocion, bool estado, bool requiereEvaluacion, bool aplicaAutomatizacion = false)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "asi_codigoP", codigo },
                { "asi_nombre", nombre },
                { "asi_abreviacion", abreviacion },
                { "asi_promocion", promocion },
                { "asi_requiere_evaluacion",requiereEvaluacion },
                { "asi_estado", estado },
                { "asi_aplica_automatizacion", aplicaAutomatizacion }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_asignaturas_colegios_modificar", parametros);
        }

        #endregion

        #region Asignaturas Pensum

        public IList<AsignaturaProgramaViewModel> ObtenerAsignaturasModular(int? programaPensum, char? ordenar = 'N')
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "modpro_propen_consecutivo", programaPensum },
                { "filtros_orden", ordenar }
            };

            return institucion.BaseDatos.ExecuteQuery<AsignaturaProgramaViewModel>("pa_aca_asignaturas_modular_retornar", parametros);
        }

        public IList<AsignaturaProgramaViewModel> ObtenerAsignaturasModularDocente(int? programaPensum, char? ordenar = 'N')
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "modpro_propen_consecutivo", programaPensum },
                { "filtros_orden", ordenar },
                { "per_codigoP", per_codigoP},
                { "rol_codigoP", rol_codigoP}
            };

            return institucion.BaseDatos.ExecuteQuery<AsignaturaProgramaViewModel>("paz_amazonia_aca_asignaturas_modular_retornar", parametros);
        }

        public IList<AsignaturaViewModel> ObtenerAsignaturasParaAsignar(int codigoPensumPrograma)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "propen_consecutivoP", codigoPensumPrograma }
            };

            return institucion.BaseDatos.ExecuteQuery<AsignaturaViewModel>("pa_aca_asignaturas_para_asignar_retornar", parametros);
        }

        public IList<AsignaturaViewModel> ObtenerAsignaturasPedagogicasParaAsignar(int codigoPensumPrograma)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "propen_consecutivoP", codigoPensumPrograma }
            };

            return institucion.BaseDatos.ExecuteQuery<AsignaturaViewModel>("pa_aca_asignaturas_pedagogicas_para_asignar_retornar", parametros);
        }

        public IList<AsignaturaProgramaViewModel> ObtenerAsignaturasPensum(int? pensum, int? modulo, char? orden = 'A', int? periodo = null)
        {
            //orden A: Asignatura, N: Nivel, O: Optativas
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "propen_consecutivoP", pensum },
                { "filtros_modular", modulo },
                { "filtros_orden", orden },
                { "filtro_periodo", periodo }
            };

            return institucion.BaseDatos.ExecuteQuery<AsignaturaProgramaViewModel>("pa_aca_asignaturas_programa_retornar", parametros);
        }

        public List<AsignaturaProgramaViewModel> ObtenerAsignaturasPensumProgramaMasivo(string consecutivosPensum)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "propen_consecutivos", consecutivosPensum }
            };

            return institucion.BaseDatos.ExecuteQuery<AsignaturaProgramaViewModel>("pa_aca_asignaturas_programa_masivo_retornar", parametros).ToList();
        }

        public IPagedList<AsignaturaProgramaViewModel> ObtenerAsignaturasPensumPaginacion(int? pensum, int? modulo, char? orden = 'A', int? periodo = null, int pageIndex = 1, int tamanioPagina = 12)
        {
            int totalCount;
            var parameters = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "propen_consecutivoP", pensum },
                { "filtros_modular", modulo },
                { "filtros_orden", orden },
                { "filtro_periodo", periodo }
            };

            var result = institucion.BaseDatos.ExecuteQuery<AsignaturaProgramaViewModel>("pa_aca_asignaturas_programa_retornar", parameters, pageIndex, tamanioPagina, out totalCount);
            return new StaticPagedList<AsignaturaProgramaViewModel>(result, pageIndex, tamanioPagina, totalCount);
        }

        public IList<AsignaturaProgramaViewModel> ObtenerAsignaturasPensumDocente(int? pensum, int? modulo, char? orden = 'A', int? periodo = null)
        {
            //orden A: Asignatura, N: Nivel
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "propen_consecutivoP", pensum },
                { "filtros_modular", modulo },
                { "filtros_orden", orden },
                { "filtro_periodo", periodo },
                { "per_codigoP", per_codigoP},
                { "rol_codigoP", rol_codigoP}
            };

            return institucion.BaseDatos.ExecuteQuery<AsignaturaProgramaViewModel>("paz_amazonia_aca_asignaturas_programa_retornar", parametros);
        }

        public IList<AsignaturaProgramaViewModel> ObtenerAsignaturasParaAsignarEquivalencia(string codigoAsignatura)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "asi_codigoP", codigoAsignatura  }
            };

            return institucion.BaseDatos.ExecuteQuery<AsignaturaProgramaViewModel>("pa_aca_asignaturas_para_asignar_equivalencias_retornar", parametros);
        }
        public List<AsignaturaProgramaViewModel> ObtenerAsignaturasPensumsMasivo(string pensums, char? pensumsActual = '1') //pensum actual = '1', pensum activo = '2'
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "propen_consecutivoP", pensums },
                { "propen_estado", pensumsActual }
            };

            return institucion.BaseDatos.ExecuteQuery<AsignaturaProgramaViewModel>("pa_aca_asignaturas_pensum_masivo_retornar", parametros).ToList();
        }

        public AsignaturaProgramaViewModel ObtenerDetalleAsignaturaPensum(int? consecutivoAsignaturaPensum)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "asipro_consecutivoP", consecutivoAsignaturaPensum },
            };

            return institucion.BaseDatos.ExecuteQuery<AsignaturaProgramaViewModel>("pa_aca_asignaturas_programa_detalles_retornar", parametros).FirstOrDefault();
        }

        public void IngresarAsignaturaPensum(string codigoAsignatura, int? codigoPensumPrograma, decimal? intensidadHoraria, string codigoNivel, decimal? numeroCreditos, bool evaluarParaCerrar, int? numeroClases, decimal? intensidadHorariaSemanal, int? modulo = null, decimal? creditosAcademicos = null, DateTime? fechaVigencia = null, int? vigencia_anios = null, int? vigencia_meses = null, int? vigencia_dias = null, string codigoOptativa = null, int? maxClasesSemanales = null, int? edadMinima = null, int? edadMaxima = null, bool? aplicaPreinscripciones = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "asipro_asi_codigo", codigoAsignatura },
                { "asipro_propen_consecutivo", codigoPensumPrograma },
                { "asipro_intensidad_horaria", intensidadHoraria },
                { "asipro_niv_codigo", codigoNivel },
                { "asipro_numero_creditos", numeroCreditos },
                { "asipro_modpro_consecutivo", modulo },
                { "asipro_evaluar_para_cerrar",evaluarParaCerrar },
                { "asipro_numero_clases", numeroClases },
                { "asipro_intensidad_horaria_semanal", intensidadHorariaSemanal },
                { "asipro_creditos_academicos", creditosAcademicos },
                { "asipro_fecha_vigencia", fechaVigencia },
                { "asipro_fecha_vigencia_anios", vigencia_anios },
                { "asipro_fecha_vigencia_meses", vigencia_meses },
                { "asipro_fecha_vigencia_dias", vigencia_dias },
                { "asipro_opt_codigo", codigoOptativa },
                { "asipro_maximo_clases_semanal", maxClasesSemanales },
                { "aud_usuario_sesion", per_codigoP },
                { "aud_ip",ClientIpAddress },
                { "aud_cat_codigo", CategoriaAuditoria },
                { "aud_usuario_q10", codigo_ejecutivo_institucion },
                { "aud_interno", false },
                //Personalización Cajicá
                { "asipro_edad_minima", edadMinima },
                { "asipro_edad_maxima", edadMaxima },
                { "asipro_aplica_preinscripcion", aplicaPreinscripciones}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_asignaturas_programas_ingresar", parametros);
        }

        public void ModificarAsignaturaPensum(int codigo, decimal? intensidadHoraria, string codigoNivel, decimal? numeroCreditos, bool evaluarParaCerrar, int? numeroClases, decimal? intensidadHorariaSemanal, decimal? creditosAcademicos = null, DateTime? fechaVigencia = null, int? vigencia_anios = null, int? vigencia_meses = null, int? vigencia_dias = null, int? maxClasesSemanales = null, int? edadMinima = null, int? edadMaxima = null, bool? aplicaPreinscripciones = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "asipro_consecutivoP", codigo },
                { "asipro_intensidad_horaria", intensidadHoraria },
                { "asipro_niv_codigo", codigoNivel },
                { "asipro_numero_creditos", numeroCreditos },
                { "asipro_evaluar_para_cerrar",evaluarParaCerrar },
                { "asipro_numero_clases", numeroClases },
                { "asipro_intensidad_horaria_semanal", intensidadHorariaSemanal },
                { "asipro_creditos_academicos", creditosAcademicos},
                { "asipro_fecha_vigencia", fechaVigencia },
                { "asipro_fecha_vigencia_anios", vigencia_anios },
                { "asipro_fecha_vigencia_meses", vigencia_meses },
                { "asipro_fecha_vigencia_dias", vigencia_dias },
                { "asipro_maximo_clases_semanal", maxClasesSemanales },
                { "aud_usuario_sesion", per_codigoP },
                { "aud_ip",ClientIpAddress },
                { "aud_cat_codigo", CategoriaAuditoria },
                { "aud_usuario_q10", codigo_ejecutivo_institucion },
                { "aud_interno", false },
                //Personalización Cajicá
                { "asipro_edad_minima", edadMinima },
                { "asipro_edad_maxima", edadMaxima },
                { "asipro_aplica_preinscripcion", aplicaPreinscripciones}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_asignaturas_programas_modificar", parametros);
        }

        public void EliminarAsignaturaPensum(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "asipro_consecutivoP", consecutivo }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_asignaturas_programas_eliminar", parametros);
        }

        public char ValidarEstudianteMatriculadoAsignatura(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "asipro_consecutivoP", consecutivo}
            };

            var result = institucion.BaseDatos.ExecuteQuery<dynamic>("pa_aca_asignaturas_programas_asignatura_validar", parametros);

            if (result.Count > 0)
            {
                return Convert.ToChar(result.First().permitir_elilimar);
            }

            return '1';
        }

        public bool ValidarEstudiantesGraduadosEgresadosPensum(int pensum)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "propen_consecutivoP", pensum}
            };

            var result = institucion.BaseDatos.ExecuteQuery<dynamic>("paz_incap_aca_pensum_graduados_egresados_validar", parametros);
            if (result.Count > 0) return Convert.ToBoolean(result.First().graduado_egresado_existente);
            return false;
        }

        /// <summary>
        /// Permite identificar si hay estudiantes matriculados para las asignaturas enviadas.
        /// </summary>
        /// <param name="consecutivos">asipro_consecutivoP de las asignaturas separados por comas (,)</param>
        /// <returns>Retorna una lista de <see cref="AsignaturaProgramaViewModel"/> con los campos asipro_asi_codigo y permitir_eliminar</returns>
        public List<AsignaturaProgramaViewModel> ValidarEstudianteMatriculadoAsignaturasMasivo(string consecutivos)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "asipro_consecutivos", consecutivos }
            };

            var result = institucion.BaseDatos.ExecuteQuery<AsignaturaProgramaViewModel>("pa_aca_asignaturas_programas_masivo_validar", parametros).ToList();
            return result;
        }

        public async Task<bool> ValidarAsignaturasIndividualesMatriculadasAPIAsync(string codigoPersona)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "per_codigoP", codigoPersona }
            };

            var response = await institucion.BaseDatos.ExecuteQueryAsync<dynamic>("pa_mov_asignaturas_individuales_persona_validar", parametros);

            if (response.Count > 0) return Convert.ToBoolean(response.First().VAL);
            return false;
        }

        #endregion Asignaturas Pensum

        #region Excepciones Asignaturas Pensum

        public IList<AsignaturaProgramaExcepcionViewModel> ObtenerExcepcionesAsignaturaPensum(int? pensum)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "propenexc_propen_consecutivo", pensum }
            };

            return institucion.BaseDatos.ExecuteQuery<AsignaturaProgramaExcepcionViewModel>("pa_aca_programas_pensum_excepciones_retornar", parametros);
        }

        public AsignaturaProgramaExcepcionViewModel ObtenerDetalleExcepcionAsignaturaPensum(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "propenexc_consecutivoP", consecutivo }
            };

            return institucion.BaseDatos.ExecuteQuery<AsignaturaProgramaExcepcionViewModel>("pa_aca_programas_pensum_excepciones_detalle_retornar", parametros).FirstOrDefault();
        }

        public void IngresarExcepcionAsignaturaPensum(string codigoAsignatura, int pensum, decimal? notaMinima, decimal? porcentajeAprobacion)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "propenexc_propen_consecutivo", pensum },
                { "propenexc_asi_codigo", codigoAsignatura },
                { "propenexc_porcentaje_aprobacion", porcentajeAprobacion },
                { "propenexc_propen_nota_aprobacion", notaMinima }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_programas_pensum_excepciones_ingresar", parametros);
        }

        public void ModificarExcepcionAsignaturaPensum(int consecutivo, decimal? notaMinima, decimal? porcentajeAprobacion)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "propenexc_consecutivoP", consecutivo },
                { "propenexc_porcentaje_aprobacion", porcentajeAprobacion },
                { "propenexc_propen_nota_aprobacion", notaMinima }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_programas_pensum_excepciones_modificar", parametros);
        }

        public void EliminarExcepcionAsignaturaPensum(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "propenexc_consecutivoP", consecutivo }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_programas_pensum_excepciones_eliminar", parametros);
        }

        #endregion

        #region Módulos Programas

        public IList<ModuloProgramaViewModel> ObtenerModulosPrograma(int? programaPensum, char? orden = 'N', bool excluirPracticas = false)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "propen_consecutivoP", programaPensum },
                { "filtros_orden", orden },// M: Módulos, N: Niveles
                { "excluir_modulo_aplica_practica", excluirPracticas },
            };

            return institucion.BaseDatos.ExecuteQuery<ModuloProgramaViewModel>("pa_aca_modulos_programas_retornar", parametros);
        }

        public IList<ModuloProgramaViewModel> ObtenerModulosParaAsignar(int? programaPensum)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "propen_consecutivoP", programaPensum }
            };

            return institucion.BaseDatos.ExecuteQuery<ModuloProgramaViewModel>("pa_aca_modulos_para_asignar_retornar", parametros);
        }

        public ModuloProgramaViewModel ObtenerDetalleModuloPrograma(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "modpro_consecutivoP", consecutivo }
            };

            return institucion.BaseDatos.ExecuteQuery<ModuloProgramaViewModel>("pa_aca_modulos_programas_detalles_retornar", parametros).FirstOrDefault();
        }

        public void IngresarModuloPrograma(int? codigoPensumPrograma, string codigoModulo, decimal? intensidadHorariaTotal, decimal? intensidadHoraria, string codigoNivel, decimal? numeroCreditos, bool evaluarParaCerrar)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "modpro_mod_codigo", codigoModulo },
                { "modpro_propen_consecutivo", codigoPensumPrograma },
                { "modpro_intensidad_horaria", intensidadHorariaTotal },
                { "modpro_intensidad_horaria_semanal", intensidadHoraria },
                { "modpro_niv_codigo", codigoNivel },
                { "modpro_numero_creditos", numeroCreditos },
                { "modpro_evaluar_para_cerrar", evaluarParaCerrar },
                { "aud_usuario_sesion", per_codigoP },
                { "aud_ip",ClientIpAddress },
                { "aud_cat_codigo", CategoriaAuditoria },
                { "aud_usuario_q10", codigo_ejecutivo_institucion },
                { "aud_interno", false }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_modulos_programas_ingresar", parametros);
        }

        public void ModificarModuloPrograma(int consecutivo, decimal? intensidadHoraria, decimal? intensidadHorariaTotal, string codigoNivel, decimal? numeroCreditos, bool evaluarParaCerrar)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "modpro_consecutivoP", consecutivo },
                { "modpro_intensidad_horaria", intensidadHoraria },
                { "modpro_intensidad_horaria_semanal", intensidadHorariaTotal },
                { "modpro_niv_codigo", codigoNivel },
                { "modpro_numero_creditos", numeroCreditos },
                { "modpro_evaluar_para_cerrar", evaluarParaCerrar },
                { "aud_usuario_sesion", per_codigoP },
                { "aud_ip",ClientIpAddress },
                { "aud_cat_codigo", CategoriaAuditoria },
                { "aud_usuario_q10", codigo_ejecutivo_institucion },
                { "aud_interno", false }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_modulos_programas_modificar", parametros);
        }

        public void EliminarModuloPrograma(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "modpro_consecutivoP", consecutivo }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_modulos_programas_eliminar", parametros);
        }

        public char ValidarEstudianteMatriculadoModulos(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "modpro_consecutivoP", consecutivo}
            };

            var result = institucion.BaseDatos.ExecuteQuery<dynamic>("pa_aca_modulos_programas_modulo_validar", parametros);

            if (result.Count > 0)
            {
                return Convert.ToChar(result.First().permitir_elilimar);
            }

            return '1';
        }

        #endregion

        #region Empresas

        public IList<PersonaViewModel> ObtenerContactosEmpresa(int empresa)
        {
            var parameters = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "emp_consecutivoP", empresa }
            };
            return institucion.BaseDatos.ExecuteQuery<PersonaViewModel>("pa_per_contacto_empresas_retornar", parameters);
        }
        public IList<PersonaViewModel> ObtenerContactosEmpresaApi(string consecutivosEmpresas)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "emp_consecutivoP", consecutivosEmpresas },
            };
            return institucion.BaseDatos.ExecuteQuery<PersonaViewModel>("pa_per_contacto_empresas_retornar", parametros);
        }

        public IList<EmpresaViewModel> ObtenerEmpresas(string texto, bool? estado)
        {
            var parameters = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_texto", texto },
                { "filtros_estado", estado }
            };

            return institucion.BaseDatos.ExecuteQuery<EmpresaViewModel>("pa_per_empresas_retornar", parameters);
        }

        public async Task<IPagedList<EmpresaViewModel>> ObtenerEmpresasAsync(string texto, bool? estado, int pagina = 1, int tamanioPagina = Constantes.PAGINACION_TAMANO_PAGINA)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_texto", texto },
                { "filtros_estado", estado }
            };

            var result = await institucion.BaseDatos.ExecuteQueryAsync<EmpresaViewModel>("pa_per_empresas_retornar", parametros, pagina, tamanioPagina);
            return new StaticPagedList<EmpresaViewModel>(result.Item1, pagina, tamanioPagina, result.Item2);
        }

        public EmpresaViewModel ObtenerDetalleEmpresa(int consecutivo)
        {
            var parameters = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "emp_consecutivoP", consecutivo }
            };

            return institucion.BaseDatos.ExecuteQuery<EmpresaViewModel>("pa_per_empresas_detalle_retornar", parameters).FirstOrDefault();
        }

        public int IngresarEmpresa
        (
            string nombre, string municipio, bool estado, string nit, string telefono, string direccion, string email, int? sector, string ejecutivoCuenta = null,
            char? categoriaAfiliacion = null, DateTime? fechaConvenio = null, string paginaWeb = null, decimal? descuentoConvenio = null, string observaciones = null,
            string fax = null, int? diasAdicionales = null
        )
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "emp_nombre", nombre },
                { "emp_mun_codigo", municipio },
                { "emp_estado", estado },
                { "emp_nit_rut", nit },
                { "emp_sec_consecutivo", sector },
                { "emp_telefono", telefono },
                { "emp_direccion", direccion },
                { "emp_email", email },
                { "emp_ejecutivo_cuenta", ejecutivoCuenta },
                { "emp_categoria_afilizacion", categoriaAfiliacion },
                { "emp_fecha_convenio", fechaConvenio },
                { "emp_pagina_web", paginaWeb },
                { "emp_descuento_convenio", descuentoConvenio },
                { "emp_observaciones", observaciones },
                { "emp_fax", fax},
                { "emp_dias_adicionales", diasAdicionales}
            };

            var id = institucion.BaseDatos.ExecuteQuery<EmpresaViewModel>("pa_per_empresas_ingresar", parametros).FirstOrDefault();

            return id.emp_consecutivoP;
        }

        public void ModificarEmpresa
        (
            int consecutivo, string nombre, string municipio, bool estado, string nit, string telefono, string direccion, string email, int? sector,
            string ejecutivoCuenta = null, char? categoriaAfiliacion = null, DateTime? fechaConvenio = null, string paginaWeb = null,
            decimal? descuentoConvenio = null, string observaciones = null, string fax = null, int? diasAdicionales = null
        )
        {
            var parameters = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "emp_consecutivoP", consecutivo },
                { "emp_nombre", nombre },
                { "emp_mun_codigo", municipio },
                { "emp_estado", estado },
                { "emp_nit_rut", nit },
                { "emp_sec_consecutivo", sector },
                { "emp_telefono", telefono },
                { "emp_direccion", direccion },
                { "emp_email", email },
                { "emp_ejecutivo_cuenta", ejecutivoCuenta },
                { "emp_categoria_afilizacion", categoriaAfiliacion },
                { "emp_fecha_convenio", fechaConvenio },
                { "emp_pagina_web", paginaWeb },
                { "emp_descuento_convenio", descuentoConvenio },
                { "emp_observaciones", observaciones },
                { "emp_fax", fax},
                { "emp_dias_adicionales", diasAdicionales}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_per_empresas_modificar", parameters);
        }

        public void EliminarEmpresa(int consecutivo)
        {
            var parameters = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "emp_consecutivoP", consecutivo }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_per_empresas_eliminar", parameters);
        }
        public EmpresaViewModel ObtenerDetalleContactoEmpresa(string codigoPersona)
        {
            var parameters = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "per_codigoP", codigoPersona }
            };

            return institucion.BaseDatos.ExecuteQuery<EmpresaViewModel>("pa_per_empresa_contactos_retornar", parameters).FirstOrDefault();
        }

        public IList<PracticaLaboralViewModel> ObtenerEstudiantesPracticaEmpresa(int empresa)
        {
            var parameters = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "emp_consecutivoP", empresa }
            };
            return institucion.BaseDatos.ExecuteQuery<PracticaLaboralViewModel>("pa_aca_practicas_laborales_estudiantes_por_empresa_retornar", parameters);
        }
        public IList<PracticaLaboralViewModel> ObtenerEstudiantesPracticaEmpresaApi(string consecutivosEmpresas)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "emp_consecutivoP", consecutivosEmpresas }
            };
            return institucion.BaseDatos.ExecuteQuery<PracticaLaboralViewModel>("pa_aca_practicas_laborales_estudiantes_por_empresa_retornar", parametros);
        }

        public IList<EstudiantesEmpresaViewModel> ObtenerEstudiantesEmpresa(bool validacion, int empresa)
        {
            var parameters = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "sin_validacion", validacion },
                { "emp_consecutivoP", empresa }
            };

            return institucion.BaseDatos.ExecuteQuery<EstudiantesEmpresaViewModel>("pa_per_estudiantes_por_empresa_retornar", parameters);
        }

        #endregion

        #region Instituciones Educativas

        public IList<InstitucionEducativaViewModel> ObtenerInstitucionesEducativas(string texto, bool? estado, string municipio = null)
        {
            var parameters = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_texto", texto },
                { "filtros_estado", estado },
                { "filtros_municipio", municipio },
            };

            return institucion.BaseDatos.ExecuteQuery<InstitucionEducativaViewModel>("pa_per_instituciones_educativas_retornar", parameters);
        }

        public async Task<IPagedList<InstitucionEducativaViewModel>> ObtenerInstitucionesEducativasApiAsync(bool? estado, int pagina, int tamanioPagina)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_estado", estado }
            };

            var result = await institucion.BaseDatos.ExecuteQueryAsync<InstitucionEducativaViewModel>("pa_apis_per_instituciones_educativas_retornar", parametros, pagina, tamanioPagina);
            return new StaticPagedList<InstitucionEducativaViewModel>(result.Item1, pagina, tamanioPagina, result.Item2);
        }

        public InstitucionEducativaViewModel ObtenerDetalleInstitucionEducativa(int consecutivo)
        {
            var parameters = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "inst_consecutivoP", consecutivo }
            };

            return institucion.BaseDatos.ExecuteQuery<InstitucionEducativaViewModel>("pa_per_instituciones_educativas_detalle_retornar", parameters).FirstOrDefault();
        }

        public int IngresarInstitucionEducativa(string nombre, string municipio, bool estado, string nit, string telefono, string direccion, string email)
        {
            var parameters = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "inst_nombre", nombre },
                { "inst_mun_codigo", municipio },
                { "inst_estado", estado },
                { "inst_nit_rut", nit },
                { "inst_telefono", telefono },
                { "inst_direccion", direccion },
                { "inst_email", email }
            };

            return institucion.BaseDatos.ExecuteNonQuery("pa_per_instituciones_educativas_ingresar", parameters);
        }

        public void ModificarInstitucionEducativa(int consecutivo, string nombre, string municipio, bool estado, string nit, string telefono, string direccion, string email)
        {
            var parameters = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "inst_consecutivoP", consecutivo },
                { "inst_nombre", nombre },
                { "inst_mun_codigo", municipio },
                { "inst_estado", estado },
                { "inst_nit_rut", nit },
                { "inst_telefono", telefono },
                { "inst_direccion", direccion },
                { "inst_email", email }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_per_instituciones_educativas_modificar", parameters);
        }

        public void EliminarInstitucionEducativa(int consecutivo)
        {
            var parameters = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "inst_consecutivoP", consecutivo }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_per_instituciones_educativas_eliminar", parameters);
        }

        public IList<InstitucionEducativaViewModel> ObtenerInstitucionesEducativasVonHumboldt(string texto, bool? estado, string municipio = null)
        {
            var parameters = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_texto", texto },
                { "filtros_estado", estado },
                { "filtros_municipio", municipio },
            };

            return institucion.BaseDatos.ExecuteQuery<InstitucionEducativaViewModel>("paz_vonhumboldt_per_instituciones_educativas_retornar", parameters);
        }

        #endregion

        #region Programas

        public IList<ProgramaViewModel> ObtenerProgramas(bool? estado, string texto, char? tipoEvaluacion = null, string categoria = null, bool desdeCrearSolicitud = false, char? modeloEvaluacion = null, bool? aplicaDescuentoRepitentes = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_estado", estado },
                { "filtros_texto", texto },
                { "filtros_tipo_evaluacion", tipoEvaluacion },
                { "per_codigoP", desdeCrearSolicitud ? null : per_codigoP },
                { "rol_codigoP", desdeCrearSolicitud ? null : rol_codigoP },
                //Campos personalización Colombo.
                { "pro_modelo_evaluacion", modeloEvaluacion },
                { "filtro_incluir_descuento", aplicaDescuentoRepitentes }
            };

            return institucion.BaseDatos.ExecuteQuery<ProgramaViewModel>("pa_aca_programas_retornar", parametros);
        }

        public async Task<IPagedList<ProgramaViewModel>> ObtenerProgramasAsync(bool? estado, string texto, int pagina, int? tamanioPagina = null, char? tipoEvaluacion = null, string categoria = null, bool desdeCrearSolicitud = false)
        {
            tamanioPagina = tamanioPagina ?? Constantes.PAGINACION_TAMANO_PAGINA;

            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_estado", estado },
                { "filtros_texto", texto },
                { "filtros_tipo_evaluacion", tipoEvaluacion },
                { "per_codigoP", desdeCrearSolicitud ? null : per_codigoP },
                { "rol_codigoP", desdeCrearSolicitud ? null : rol_codigoP },
            };

            var (programas, cantidadTotal) = await institucion.BaseDatos.ExecuteQueryAsync<ProgramaViewModel>("pa_aca_programas_retornar", parametros, pagina, tamanioPagina.Value);
            return new StaticPagedList<ProgramaViewModel>(programas, pagina, tamanioPagina.Value, cantidadTotal);
        }

        public async Task<IList<ProgramaViewModel>> ObtenerProgramasAsync(bool? estado, string texto, char? tipoEvaluacion = null, string categoria = null, bool desdeCrearSolicitud = false)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_estado", estado },
                { "filtros_texto", texto },
                { "filtros_tipo_evaluacion", tipoEvaluacion },
                { "per_codigoP", desdeCrearSolicitud ? null : per_codigoP },
                { "rol_codigoP", desdeCrearSolicitud ? null : rol_codigoP },
            };

            return await institucion.BaseDatos.ExecuteQueryAsync<ProgramaViewModel>("pa_aca_programas_retornar", parametros);
        }

        public ProgramaViewModel ObtenerDetallePrograma(string codigo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "pro_codigoP", codigo }
            };

            return institucion.BaseDatos.ExecuteQuery<ProgramaViewModel>("pa_aca_programas_detalle_retornar", parametros).FirstOrDefault();
        }

        public async Task<ProgramaViewModel> ObtenerDetalleProgramaAsync(string codigo, bool? estado = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "pro_codigoP", codigo },
                { "pro_estado", estado }
            };

            return (await institucion.BaseDatos.ExecuteQueryAsync<ProgramaViewModel>("pa_aca_programas_detalle_retornar", parametros)).FirstOrDefault();
        }

        public void IngresarPrograma(string codigo, string nombre, string abreviacion, string numeroResolucion, string tipo_programa, DateTime? fechaResolucion, bool aplicaPreinscripcion, bool estado, bool aplicaGrupo, char? tipoEvaluacion, int consecutivoCategoriaPrograma, string variable = null,
            string numeroResolucionRevalidacion = null, DateTime? fechaResolucionRevalidacion = null, bool? tecnicoLaboral = null, decimal? numeroCreditos = null, string codigoSnies = null, string facultad = null, string producto = null, char? aplicaPara = null,
            string categoria = null, string codigo_super = null, int? intensidad_horaria_aprobada = null, int? intensidad_horaria_practicas = null, string centroCostos = null, bool? aplica_descuento_graduados = null, char? tipo_programa_incap = null, char? tipoPreinscripcion = null,
            char? tipoProgramaCajica = null, bool? aplicaCertificadoQuality = null, bool aplicaMatriculaCursos = false, int? tipoPrograma = null, decimal? creditoRenovacion = null, decimal? creditosMinimos = null, int? diasLimiteNivelacion = null, int? consecutivoDescuento = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "pro_codigoP", codigo },
                { "pro_nombre", nombre },
                { "pro_abreviatura", abreviacion },
                { "pro_numero_resolucion", numeroResolucion },
                { "pro_fecha_resolucion", fechaResolucion },
                { "pro_aplica_preinscripcion", aplicaPreinscripcion },
                { "pro_aplica_grupo", aplicaGrupo },
                { "pro_estado", estado },
                { "pro_tipo_evaluacion", tipoEvaluacion },
                { "pro_clapro_consecutivo", consecutivoCategoriaPrograma},
                { "pro_variable1", variable },
                { "pro_numero_resolucion_revalidacion", numeroResolucionRevalidacion },
                { "pro_fecha_resolucion_revalidacion", fechaResolucionRevalidacion },
                { "pro_tecnico_laboral", tecnicoLaboral },
                { "pro_numero_creditos", numeroCreditos },
                { "pro_codigo_snies", codigoSnies },
                { "pro_fac_codigo", facultad },
                { "pro_prod_codigo", producto },
                { "pro_codigo_super", codigo_super },
                //{ "pro_clapro_consecutivo", clasificacion },
                //Campo Personalizado Unilatiina
                { "pro_aplica_para", aplicaPara },
                { "pro_tippro_codigo", tipo_programa },
                //Campo para Cafam Cet
                { "pro_categoria", categoria },
                //Personalización Incap
                { "pro_intensidad_horaria_aprobada", intensidad_horaria_aprobada },
                { "pro_intensidad_horaria_practicas", intensidad_horaria_practicas },
                //Personalización Pachacutec
                { "pro_centro_costos", centroCostos },
                //Personalizacion incap programas, descuento graduados, tipo programa regularo de ingles
                { "pro_aplica_descuento_graduados", aplica_descuento_graduados },
                { "pro_tipo", tipo_programa_incap },
                { "pro_tipo_preinscripcion", tipoPreinscripcion },
                //Personalización cajica/comfaboy/quality tipo de programa
                { "pro_tipo_programa", tipoProgramaCajica },
                //Personalización Quality
                { "pro_aplica_certificado", aplicaCertificadoQuality },
                //Comfamiliar
                { "pro_aplica_matricula_cursos", aplicaMatriculaCursos },
                { "pro_tippro_consecutivo", tipoPrograma },
                //Personalizacion DNA
                { "pro_creditos_renovacion", creditoRenovacion },
                { "pro_cantidad_minima_creditos", creditosMinimos },
                //Personalizacion Colombo
                { "pro_dias_nivelacion", diasLimiteNivelacion },
                { "pro_des_consecutivo", consecutivoDescuento },
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_programas_ingresar", parametros);
        }

        public void ModificarPrograma(string codigo, string nombre, string abreviacion, string numeroResolucion, string tipo_programa, DateTime? fechaResolucion, bool aplicaPreinscripcion, bool estado, bool aplicaGrupo, int consecutivoCategoriaPrograma, string variable = null,
            string numeroResolucionRevalidacion = null, DateTime? fechaResolucionRevalidacion = null, bool? tecnicoLaboral = null, decimal? numeroCreditos = null, string codigoSnies = null, string facultad = null, string producto = null, char? aplicaPara = null,
            string categoria = null, string codigo_super = null, int? intensidad_horaria_aprobada = null, int? intensidad_horaria_practicas = null, string centroCostos = null, bool? aplica_descuento_graduados = null, char? tipo_programa_incap = null,
            char? tipoPreinscripcion = null, char? tipoProgramaCajica = null, bool? aplicaCertificadoQuality = null, bool aplicaMatriculaCursos = false, int? tipoPrograma = null, decimal? creditoRenovacion = null, decimal? creditosMinimos = null, int? diasLimiteNivelacion = null,
            int? consecutivoDescuento = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "pro_codigoP", codigo },
                { "pro_nombre", nombre },
                { "pro_abreviatura", abreviacion },
                { "pro_numero_resolucion", numeroResolucion },
                { "pro_fecha_resolucion", fechaResolucion },
                { "pro_aplica_preinscripcion", aplicaPreinscripcion },
                { "pro_aplica_grupo", aplicaGrupo },
                { "pro_clapro_consecutivo", consecutivoCategoriaPrograma },
                { "pro_estado", estado },
                { "pro_variable1", variable },
                { "pro_numero_resolucion_revalidacion", numeroResolucionRevalidacion },
                { "pro_fecha_resolucion_revalidacion", fechaResolucionRevalidacion },
                { "pro_tecnico_laboral", tecnicoLaboral },
                { "pro_numero_creditos", numeroCreditos },
                { "pro_codigo_snies", codigoSnies },
                { "pro_fac_codigo", facultad },
                { "pro_prod_codigo", producto },
                { "pro_codigo_super", codigo_super },
                //{ "pro_clapro_consecutivo", clasificacion },
                //Campo Personalizado Unilatiina
                { "pro_aplica_para", aplicaPara },
                { "pro_tippro_codigo", tipo_programa },
                //Campo para Cafam Cet
                { "pro_categoria", categoria },
                //Personalización Incap
                { "pro_intensidad_horaria_aprobada", intensidad_horaria_aprobada },
                { "pro_intensidad_horaria_practicas", intensidad_horaria_practicas },
                //Personalización Pachacutec
                { "pro_centro_costos", centroCostos },
                //Personalizacion incap programas, descuento graduados, tipo programa regularo de ingles
                { "pro_aplica_descuento_graduados", aplica_descuento_graduados },
                { "pro_tipo", tipo_programa_incap },
                { "pro_tipo_preinscripcion", tipoPreinscripcion },
                //Personalización cajica tipo de programa
                { "pro_tipo_programa", tipoProgramaCajica },
                //Personalización Quality
                { "pro_aplica_certificado", aplicaCertificadoQuality },
                //Comfamiliar
                { "pro_aplica_matricula_cursos", aplicaMatriculaCursos },
                { "pro_tippro_consecutivo", tipoPrograma },
                //Personalización Colombo
                { "pro_dias_nivelacion", diasLimiteNivelacion },
                { "pro_des_consecutivo", consecutivoDescuento },
                { "aud_usuario_sesion", per_codigoP },
                { "aud_ip",ClientIpAddress },
                { "aud_cat_codigo", CategoriaAuditoria },
                { "aud_usuario_q10", codigo_ejecutivo_institucion },
                { "aud_interno", false },
                //Personalizacion DNA
                { "pro_creditos_renovacion", creditoRenovacion},
                { "pro_cantidad_minima_creditos", creditosMinimos }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_programas_modificar", parametros);
        }
        public void EliminarPrograma(string codigo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "pro_codigoP", codigo }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_programas_eliminar", parametros);
        }

        public CantidadCreditosProgramaViewModel ObtenerCreditosProgramaPersona(int matriculaPrograma)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "matpro_consecutivoP", matriculaPrograma }
            };

            return institucion.BaseDatos.ExecuteQuery<CantidadCreditosProgramaViewModel>("pa_aca_cantidad_de_creditos_por_estudiante_retornar", parametros).FirstOrDefault();
        }

        public IList<ProgramaSedeJornadaViewModel> ObtenerOpcionesProgramasPreinscripciones(int? consecutivoSedeJornada, string tipoPrograma = null)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP",institucion.aplent_codigoP },
                { "sedjor_consecutivoP", consecutivoSedeJornada },
                { "tippro_codigoP", tipoPrograma }
            };

            return institucion.BaseDatos.ExecuteQuery<ProgramaSedeJornadaViewModel>("pa_par_programas_preinscripciones_retornar", parametros);
        }

        #endregion Programas

        #region Modulos

        public IList<ModuloViewModel> ObtenerModulos(bool? estado, string texto)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_estado", estado },
                { "filtros_texto", texto }
            };

            return institucion.BaseDatos.ExecuteQuery<ModuloViewModel>("pa_aca_modulos_retornar", parametros);
        }

        public IList<ModuloViewModel> ObtenerModulosFiltro()
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "per_codigoP", per_codigoP},
                { "rol_codigoP", rol_codigoP}
            };

            return institucion.BaseDatos.ExecuteQuery<ModuloViewModel>("pa_aca_modulos_retornar", parametros);
        }

        public ModuloViewModel ObtenerDetalleModulo(string codigo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "mod_codigoP", codigo }
            };

            return institucion.BaseDatos.ExecuteQuery<ModuloViewModel>("pa_aca_modulos_detalle_retornar", parametros).FirstOrDefault();
        }

        public void IngresarModulo(string codigo, string nombre, string abreviacion, bool estado, char? tipo, bool practica)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "mod_codigoP", codigo },
                { "mod_nombre", nombre },
                { "mod_abreviacion", abreviacion },
                { "mod_estado", estado },
                { "mod_tipo", tipo },
                { "mod_practicas", practica}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_modulos_ingresar", parametros);
        }

        public void ModificarModulo(string codigo, string nombre, string abreviacion, bool estado, char? tipo, bool practica)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "mod_codigoP", codigo },
                { "mod_nombre", nombre },
                { "mod_abreviacion", abreviacion },
                { "mod_estado", estado },
                { "mod_tipo", tipo },
                { "mod_practicas", practica}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_modulos_modificar", parametros);
        }

        public void EliminarModulo(string codigo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "mod_codigoP", codigo}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_modulos_eliminar", parametros);
        }

        public IList<ModuloViewModel> ObtenerModuloParaAsignarPracticas(int matriculaPrograma)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "matpro_consecutivoP", matriculaPrograma}
            };

            return institucion.BaseDatos.ExecuteQuery<ModuloViewModel>("pa_aca_modulos_para_asignar_practicas_laboral_retornar", parametros);
        }

        public List<ModuloPadreViewModel> ObtenerModulosPadreSedeJornada(string texto, int? sedeJornada, int? periodo = null)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP",institucion.aplent_codigoP },
                { "filtros_texto", texto },
                { "sedjor_consecutivoP", sedeJornada },
                { "per_consecutivoP", periodo }
            };

            return institucion.BaseDatos.ExecuteQuery<ModuloPadreViewModel>("pa_aca_modulos_padre_retornar", parametros).ToList();
        }

        public List<ModuloProgramaViewModel> ObtenerModuloPensumsMasivo(string pensums)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "propen_consecutivos", pensums }
            };

            // Se retornan solo asignaturas de pensum actuales.
            return institucion.BaseDatos.ExecuteQuery<ModuloProgramaViewModel>("pa_aca_modulos_pensum_masivo_retornar", parametros).ToList();
        }

        #endregion

        #region Grados

        public IList<GradoViewModel> ObtenerGrados(bool? estado, string texto, string codigoNivel, char? filtro = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_estado", estado },
                { "filtros_texto", texto },
                { "gra_niv_codigo", codigoNivel },
                { "rol_codigoP", rol_codigoP },
                { "per_codigoP", per_codigoP },
                { "filtro_tipo", filtro }
            };

            return institucion.BaseDatos.ExecuteQuery<GradoViewModel>("pa_aca_grados_retornar", parametros);
        }

        public async Task<IList<GradoViewModel>> ObtenerGradosAsync(bool? estado, string texto, string codigoNivel, char? filtro = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_estado", estado },
                { "filtros_texto", texto },
                { "gra_niv_codigo", codigoNivel },
                { "rol_codigoP", rol_codigoP },
                { "per_codigoP", per_codigoP },
                { "filtro_tipo", filtro }
            };

            return await institucion.BaseDatos.ExecuteQueryAsync<GradoViewModel>("pa_aca_grados_retornar", parametros);
        }

        public IList<GradoViewModel> ObtenerDirectorGradosGrupo(bool? estado)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_estado", estado },
                { "per_codigoP", per_codigoP },
                { "rol_codigoP", rol_codigoP }
            };

            return institucion.BaseDatos.ExecuteQuery<GradoViewModel>("pa_aca_coordinadores_grupos_retornar", parametros);
        }

        public GradoViewModel ObtenerDetalleGrado(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gra_consecutivoP", consecutivo }
            };

            return institucion.BaseDatos.ExecuteQuery<GradoViewModel>("pa_aca_grados_detalle_retornar", parametros).FirstOrDefault();
        }

        public int? ObtenerNumeroMaximoIndicadoresLogroGrado(int codigoPlanAcademico)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaaca_consecutivoP", codigoPlanAcademico }
            };

            var result = institucion.BaseDatos.ExecuteQuery<GradoViewModel>("pa_aca_matriculas_planes_indicadores_logros_maximo_grado_retornar", parametros).FirstOrDefault();
            return result != null ? result.gra_numero_indicadores : 0;
        }

        public int IngresarGrado(string codigo, string nombre, string nivel, int? porcentajeInasistencia, char sistemaEducacion, int? numeroIndicadores, bool estado, int? numeroAreasReprobar,
            int? ordenamiento, int? edadMinima, int? edadMaxima, decimal? notaMinima, decimal? notaMinimaEntrevista, DateTime? fechaLimite, DateTime? fechaInicioInscripciones, DateTime? fechaLimiteFormularioAdmision,
            bool? aplicaGraduacion, bool? aplicaCalificacionCualitativa, string cuentaContableCategoria = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gra_codigo", codigo },
                { "gra_nombre", nombre },
                { "gra_niv_codigo", nivel },
                { "gra_porcentaje_inasistencia", porcentajeInasistencia },
                { "gra_sistema_educacion", sistemaEducacion },
                { "gra_numero_indicadores", numeroIndicadores },
                { "gra_numero_areas_para_reprobacion", numeroAreasReprobar },
                { "gra_estado", estado },
                { "gra_orden", ordenamiento },
                { "gra_edad_minima", edadMinima },
                { "gra_edad_maxima", edadMaxima },
                { "gra_nota_minima_admision", notaMinima },
                { "gra_nota_minima_entrevista", notaMinimaEntrevista },
                { "gra_fecha_limite_preinscripcion", fechaLimite },
                { "gra_fecha_inicio_preinscripcion" ,fechaInicioInscripciones },
                { "gra_fecha_limite_formulario_admision", fechaLimiteFormularioAdmision },
                { "gra_aplica_graduacion", aplicaGraduacion },
                { "gra_calificacion_cualitativa", aplicaCalificacionCualitativa },
                { "gra_cuenta_contable_categoria", cuentaContableCategoria}
            };

            var id = institucion.BaseDatos.ExecuteQuery<GradoViewModel>("pa_aca_grados_ingresar", parametros).FirstOrDefault();
            return id.gra_consecutivoP;
        }

        public void ModificarGrado(int consecutivo, string codigo, string nombre, string nivel, int? porcentajeInasistencia, char sistemaEducacion, int? numeroIndicadores, bool estado,
            int? numeroAreasReprobar, int? ordenamiento, int? edadMinima, int? edadMaxima, decimal? notaMinima, decimal? notaMinimaEntrevista, DateTime? fechaLimite, DateTime? fechaInicioInscripciones,
            DateTime? fechaLimiteFormularioAdmision, bool? aplicaGraduacion, bool? aplicaCalificacionCualitativa, string cuentaContableCategoria = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gra_consecutivoP", consecutivo },
                { "gra_codigo", codigo },
                { "gra_nombre", nombre },
                { "gra_niv_codigo", nivel },
                { "gra_porcentaje_inasistencia", porcentajeInasistencia },
                { "gra_sistema_educacion", sistemaEducacion },
                { "gra_numero_indicadores", numeroIndicadores },
                { "gra_numero_areas_para_reprobacion", numeroAreasReprobar },
                { "gra_estado", estado },
                { "gra_orden", ordenamiento },
                { "gra_edad_minima", edadMinima },
                { "gra_edad_maxima", edadMaxima },
                { "gra_nota_minima_admision ", notaMinima },
                { "gra_nota_minima_entrevista", notaMinimaEntrevista },
                { "gra_fecha_limite_preinscripcion", fechaLimite },
                { "gra_fecha_inicio_preinscripcion", fechaInicioInscripciones },
                { "gra_fecha_limite_formulario_admision", fechaLimiteFormularioAdmision },
                { "gra_aplica_graduacion", aplicaGraduacion },
                { "gra_calificacion_cualitativa", aplicaCalificacionCualitativa },
                { "gra_cuenta_contable_categoria", cuentaContableCategoria}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_grados_modificar", parametros);
        }

        public void EliminarGrado(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gra_consecutivoP", consecutivo }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_grados_eliminar", parametros);
        }

        #region API Colegios - Grados       

        public async Task<IPagedList<GradoViewModel>> ObtenerGradosApiAsync(string codigoNivel, bool? estado = null, int pagina = 1, int tamanioPagina = 12)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gra_niv_codigo", codigoNivel },
                { "gra_estado ", estado }
            };

            var result = await institucion.BaseDatos.ExecuteQueryAsync<GradoViewModel>("pa_apis_aca_grados_retornar", parametros, pagina, tamanioPagina);
            return new StaticPagedList<GradoViewModel>(result.Item1, pagina, tamanioPagina, result.Item2);
        }

        public async Task<GradoViewModel> ObtenerDetalleGradoApiAsync(int consecutivoGrado, bool? estado = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gra_consecutivoP", consecutivoGrado },
                { "gra_estado", estado }
            };

            return (await institucion.BaseDatos.ExecuteQueryAsync<GradoViewModel>("pa_apis_aca_grados_detalle_retornar", parametros)).FirstOrDefault();
        }

        public async Task<GradoViewModel> ObtenerDetalleBasicoGradoAsync(string codigoGrado, bool? estado = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gra_codigo", codigoGrado },
                { "gra_estado", estado }
            };

            return (await institucion.BaseDatos.ExecuteQueryAsync<GradoViewModel>("pa_aca_grado_detalle_basico_retornar", parametros)).FirstOrDefault();
        }

        public void IngresarGradoApi(string codigo, string nombre, string nivel, char? sistemaEducacion, int? numeroIndicadores, int? porcentajeInasistencia, int? numeroAreasReprobar, int? ordenamiento,
            bool? estadoGrado, bool? aplicaCalificacionCualitativa, bool? aplicaGraduacion)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gra_codigo", codigo },
                { "gra_nombre", nombre },
                { "gra_niv_codigo", nivel },
                { "gra_sistema_educacion", sistemaEducacion },
                { "gra_numero_indicadores", numeroIndicadores },
                { "gra_porcentaje_inasistencia", porcentajeInasistencia },
                { "gra_numero_areas_para_reprobacion", numeroAreasReprobar },
                { "gra_orden", ordenamiento },
                { "gra_estado", estadoGrado },
                { "gra_calificacion_cualitativa", aplicaCalificacionCualitativa },
                { "gra_aplica_graduacion", aplicaGraduacion }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_apis_aca_grados_ingresar", parametros);
        }

        #endregion API Colegios - Grados

        #endregion

        #region Periodos - Grados

        public IList<PeriodoGradoViewModel> ObtenerPeriodosGrados(int? grado, int? periodo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "graper_gra_consecutivo", grado },
                { "graper_per_consecutivo", periodo }
            };

            return institucion.BaseDatos.ExecuteQuery<PeriodoGradoViewModel>("pa_aca_grados_periodos_parametros_retornar", parametros);
        }

        public async Task<IList<PeriodoGradoViewModel>> ObtenerPeriodosGradosAsync(int? grado, int? periodo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "graper_gra_consecutivo", grado },
                { "graper_per_consecutivo", periodo }
            };

            return (await institucion.BaseDatos.ExecuteQueryAsync<PeriodoGradoViewModel>("pa_aca_grados_periodos_parametros_retornar", parametros)).ToList();
        }

        public async Task<IPagedList<PeriodoGradoViewModel>> ObtenerPeriodosGradosAsyncPaginado(int? grado, int? periodo, int pagina, int tamanioPagina)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "graper_gra_consecutivo", grado },
                { "graper_per_consecutivo", periodo }
            };

            var (periodosGrados, cantidadTotalElementos) = await institucion.BaseDatos.ExecuteQueryAsync<PeriodoGradoViewModel>("pa_aca_grados_periodos_parametros_retornar", parametros, pagina, tamanioPagina);
            return new StaticPagedList<PeriodoGradoViewModel>(periodosGrados, pagina, tamanioPagina, cantidadTotalElementos);
        }

        public async Task<IList<PeriodoPorcentajeViewModel>> ObtenerGradosPeriodosApiAsync(string consecutivosPlanesAcademicos = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaaca_consecutivos", consecutivosPlanesAcademicos },
            };

            return await institucion.BaseDatos.ExecuteQueryAsync<PeriodoPorcentajeViewModel>("pa_api_aca_planes_academicos_periodos_retornar", parametros);
        }

        public PeriodoGradoViewModel ObtenerDetallePeriodoGrado(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "graper_consecutivoP", consecutivo },
            };

            return institucion.BaseDatos.ExecuteQuery<PeriodoGradoViewModel>("pa_aca_grados_periodos_parametros_detalle_retornar", parametros).FirstOrDefault();
        }
        
        public async Task<PeriodoGradoViewModel> ObtenerDetallePeriodoGradoAsync(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "graper_consecutivoP", consecutivo },
            };

            return (await institucion.BaseDatos.ExecuteQueryAsync<PeriodoGradoViewModel>("pa_aca_grados_periodos_parametros_detalle_retornar", parametros)).FirstOrDefault();
        }

        public void IngresarPeriodoGrado(string nombre, int? porcentaje, int grado, int periodo, int? numero, DateTime? fechaInicioPeriodo, DateTime? fechaFinPeriodo, DateTime? fechaInicioEvaluacion, DateTime? fechaFinEvaluacion, DateTime? fechaInicioRecuperacion, DateTime? fechaFinRecuperacion)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "graper_gra_consecutivo", grado },
                { "graper_per_consecutivo", periodo },
                { "graper_nombre", nombre },
                { "graper_porcentaje", porcentaje },
                { "graper_numero", numero },
                { "graper_fecha_inicio", fechaInicioPeriodo },
                { "graper_fecha_fin", fechaFinPeriodo },
                { "graper_fecha_inicio_evaluacion", fechaInicioEvaluacion },
                { "graper_fecha_fin_evaluacion", fechaFinEvaluacion },
                { "graper_fecha_inicio_recuperacion", fechaInicioRecuperacion },
                { "graper_fecha_fin_recuperacion", fechaFinRecuperacion }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_grados_periodos_parametros_ingresar", parametros);
        }

        public async Task<int> IngresarPeriodoGradoAsync(string nombre, int? porcentaje, int grado, int periodo, int? numero, DateTime? fechaInicioPeriodo, DateTime? fechaFinPeriodo, DateTime? fechaInicioEvaluacion, DateTime? fechaFinEvaluacion, DateTime? fechaInicioRecuperacion, DateTime? fechaFinRecuperacion, char? origenAuditoria = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "graper_gra_consecutivo", grado },
                { "graper_per_consecutivo", periodo },
                { "graper_nombre", nombre },
                { "graper_porcentaje", porcentaje },
                { "graper_numero", numero },
                { "graper_fecha_inicio", fechaInicioPeriodo },
                { "graper_fecha_fin", fechaFinPeriodo },
                { "graper_fecha_inicio_evaluacion", fechaInicioEvaluacion },
                { "graper_fecha_fin_evaluacion", fechaFinEvaluacion },
                { "graper_fecha_inicio_recuperacion", fechaInicioRecuperacion },
                { "graper_fecha_fin_recuperacion", fechaFinRecuperacion },
                { "aud_origen", origenAuditoria },
                { "aud_usuario_sesion", per_codigoP },
                { "aud_ip",ClientIpAddress },
                { "aud_cat_codigo", CategoriaAuditoria },
                { "aud_usuario_q10", codigo_ejecutivo_institucion },
            };

            return await institucion.BaseDatos.ExecuteNonQueryAsync("pa_aca_grados_periodos_parametros_ingresar", parametros);
        }

        public void ModificarPeriodoGrado(int consecutivo, string nombre, int? porcentaje, int? numero, DateTime? fechaInicioPeriodo, DateTime? fechaFinPeriodo, DateTime? fechaInicioEvaluacion, DateTime? fechaFinEvaluacion, DateTime? fechaInicioRecuperacion, DateTime? fechaFinRecuperacion)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "graper_consecutivoP", consecutivo },
                { "graper_nombre", nombre },
                { "graper_porcentaje", porcentaje },
                { "graper_numero", numero },
                { "graper_fecha_inicio", fechaInicioPeriodo },
                { "graper_fecha_fin", fechaFinPeriodo },
                { "graper_fecha_inicio_evaluacion", fechaInicioEvaluacion },
                { "graper_fecha_fin_evaluacion", fechaFinEvaluacion },
                { "graper_fecha_inicio_recuperacion", fechaInicioRecuperacion },
                { "graper_fecha_fin_recuperacion", fechaFinRecuperacion },
                { "aud_usuario_sesion", per_codigoP },
                { "aud_ip",ClientIpAddress },
                { "aud_cat_codigo", CategoriaAuditoria },
                { "aud_usuario_q10", codigo_ejecutivo_institucion },
                { "aud_interno", false }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_grados_periodos_parametros_modificar", parametros);
        }

        public void EliminarPeriodoGrado(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "graper_consecutivoP", consecutivo },
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_grados_periodos_parametros_eliminar", parametros);
        }

        #region Personalizaciones CAFAM

        public IList<ExcepcionesDocenteViewModel> ObtenerExcepcionesDocente(int? periodo, char? tipo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "graperres_graper_consecutivo", periodo },
                { "graperres_tipo", tipo }
            };

            return institucion.BaseDatos.ExecuteQuery<ExcepcionesDocenteViewModel>("pa_aca_grados_periodos_restricciones_excepciones_retornar", parametros);
        }

        public ExcepcionesDocenteViewModel ObtenerDetalleExcepcionesDocente(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "graperres_consecutivoP", consecutivo },
            };

            return institucion.BaseDatos.ExecuteQuery<ExcepcionesDocenteViewModel>("pa_aca_grados_periodos_restricciones_excepciones_detalle_retornar", parametros).FirstOrDefault();
        }

        public void IngresarExcepcionesDocente(int gradoPeriodo, string docente, DateTime? fechaInicio, DateTime? fechaFin, char? tipo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "graperres_graper_consecutivo", gradoPeriodo },
                { "graperres_per_codigo", docente },
                { "graperres_fecha_inicio", fechaInicio },
                { "graperres_fecha_fin", fechaFin },
                { "graperres_tipo", tipo },
                { "aud_usuario_sesion", per_codigoP },
                { "aud_ip", ClientIpAddress },
                { "aud_cat_codigo", CategoriaAuditoria },
                { "aud_usuario_q10", codigo_ejecutivo_institucion },
                { "aud_interno", false }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_grados_periodos_restricciones_excepciones_ingresar", parametros);
        }

        public void ModificarExcepcionesDocente(int consecutivo, DateTime? fechaInicio, DateTime? fechaFin)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "graperres_consecutivoP", consecutivo },
                { "graperres_fecha_inicio", fechaInicio },
                { "graperres_fecha_fin", fechaFin },
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_tbl_aca_grados_periodos_restricciones_excepciones_modificar", parametros);
        }

        public void EliminarExcepcionesDocente(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "graperres_consecutivoP", consecutivo },
                { "aud_usuario_sesion", per_codigoP },
                { "aud_ip", ClientIpAddress },
                { "aud_cat_codigo", CategoriaAuditoria },
                { "aud_usuario_q10", codigo_ejecutivo_institucion },
                { "aud_interno", false }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_grados_periodos_restricciones_excepciones_eliminar", parametros);
        }

        #endregion

        #endregion        

        #region Cierre Colegios

        public IList<PeriodoCerrarViewModel> ObtenerPeriodosGradoParaCerrar(int? consecutivoSedeJornada)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP",institucion.aplent_codigoP },
                { "sejor_consecutivoP", consecutivoSedeJornada}
            };

            return institucion.BaseDatos.ExecuteQuery<PeriodoCerrarViewModel>("pa_aca_grados_periodos_por_sede_jornada_retornar", parametros);
        }

        public IList<GrupoPendienteViewModel> ValidarCerrarPeriodo(int? gradoPeriodo)
        {
            var parameters = new Dictionary<string, object>()
            {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "graper_consecutivoP", gradoPeriodo }
            };

            return institucion.BaseDatos.ExecuteQuery<GrupoPendienteViewModel>("pa_aca_cierres_grados_periodos_validar_para_cerrar", parameters);
        }

        public void AbrirCerrarPeriodo(int? gradoPeriodo, bool cerrar = true, int? sedeJornada = null)
        {
            var parameters = new Dictionary<string, object>()
            {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "graper_consecutivoP", gradoPeriodo },
                { "graper_cerrado",cerrar },
                { "aud_usuario_sesion", per_codigoP },
                { "aud_ip",ClientIpAddress },
                { "aud_cat_codigo", CategoriaAuditoria },
                { "aud_usuario_q10", codigo_ejecutivo_institucion },
                { "aud_interno", false },
                { "sedjorprogra_consecutivoP", sedeJornada}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_cierres_grados_periodos_cerrar_abrir", parameters);
        }

        public IPagedList<EstadoPromocionViewModel> ObtenerEstadoPromocion(int sedeJornadaGrado, int periodo, int? grupo, bool? promovido, int pageIndex)
        {
            int totalCount;
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "sedjorprogra_consecutivoP", sedeJornadaGrado },
                { "per_consecutivoP", periodo },
                { "gru_consecutivoP", grupo },
                { "matestpro_promovido", promovido }
            };

            var result = institucion.BaseDatos.ExecuteQuery<EstadoPromocionViewModel>("pa_aca_cierres_grados_matriculas_colegios_estado_promocion_retornar", parametros, pageIndex, 10, out totalCount);
            return new StaticPagedList<EstadoPromocionViewModel>(result, pageIndex, 10, totalCount);
        }

        public void EstablecerEstadoPromocion(int sedeJornadaGrado, int periodo)
        {
            var parameters = new Dictionary<string, object>()
            {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "sedjorprogra_consecutivoP", sedeJornadaGrado },
                { "per_consecutivoP", periodo },
                { "aud_usuario_sesion", per_codigoP },
                { "aud_ip",ClientIpAddress },
                { "aud_cat_codigo", CategoriaAuditoria },
                { "aud_usuario_q10", codigo_ejecutivo_institucion },
                { "aud_interno", false }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_cierres_grados_establecer_estado_promocion_estudiantes", parameters);
        }

        public void ReversarEstadoPromocion(int sedeJornadaGrado, int periodo)
        {
            var parameters = new Dictionary<string, object>()
            {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "sedjorprogra_consecutivoP", sedeJornadaGrado },
                { "per_consecutivoP", periodo },
                { "aud_usuario_sesion", per_codigoP },
                { "aud_ip",ClientIpAddress },
                { "aud_cat_codigo", CategoriaAuditoria }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_cierres_grados_establecer_estado_promocion_estudiantes_reversar", parameters);
        }

        public void ModificarEstadoPromocion(int sedeJornadaGrado, int periodo, bool promovido, string matricula)
        {
            var parameters = new Dictionary<string, object>()
            {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "sedjorprogra_consecutivoP", sedeJornadaGrado },
                { "per_consecutivoP", periodo },
                { "mat_codigoP", matricula },
                { "matestpro_promovido", promovido },
                { "aud_usuario_sesion", per_codigoP },
                { "aud_ip",ClientIpAddress },
                { "aud_cat_codigo", CategoriaAuditoria },
                { "aud_usuario_q10", codigo_ejecutivo_institucion },
                { "aud_interno", false }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_cierres_grados_promocion_flexible_ingresar", parameters);
        }

        public PeriodoCerrarViewModel ObtenerTotalEstadoPromocion(int sedeJornadaGrado, int periodo)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP",institucion.aplent_codigoP },
                { "sedjorprogra_consecutivoP", sedeJornadaGrado },
                { "per_consecutivoP", periodo}
            };

            return institucion.BaseDatos.ExecuteQuery<PeriodoCerrarViewModel>("pa_aca_cierres_grados_matriculas_colegios_estado_promocion_totales_retornar", parametros).FirstOrDefault();
        }

        public string ArchivarGrado(int sedeJornadaGrado, int periodo)
        {
            var parameters = new Dictionary<string, object>()
            {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "sedjorprogra_consecutivoP", sedeJornadaGrado },
                { "per_consecutivoP", periodo },
                { "aud_usuario_sesion", per_codigoP },
                { "aud_ip",ClientIpAddress },
                { "aud_cat_codigo", CategoriaAuditoria },
                { "aud_usuario_q10", codigo_ejecutivo_institucion },
                { "aud_interno", false }
            };

            var result = institucion.BaseDatos.ExecuteQuery<dynamic>("pa_aca_cierres_grados_archivar", parameters).FirstOrDefault();
            if (result == null) return string.Empty;

            return result.per_codigoP;
        }

        public IList<EstudianteRecuperacionViewModel> ObtenerEstudiantesRecuperacion(int sedeJornadaGrado, int periodo, int? grupo, string asignatura)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "sedjorprogra_consecutivoP", sedeJornadaGrado },
                { "per_consecutivoP", periodo },
                { "gru_consecutivoP", grupo },
                { "asi_codigoP", asignatura }
            };

            return institucion.BaseDatos.ExecuteQuery<EstudianteRecuperacionViewModel>("pa_aca_cierres_grados_matriculas_para_recuperar_retornar", parametros);
        }

        public IList<dynamic> ObtenerEstudiantesGradoPeriodo(int? gradoPeriodo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "graper_consecutivoP", gradoPeriodo }
            };

            return institucion.BaseDatos.ExecuteQuery<dynamic>("pa_aca_cierres_grados_matriculas_colegios_estudiantes_retornar", parametros);
        }

        public IList<GrupoPendienteViewModel> ValidarGrupoSinPlanesAcademicos(int? grado, int? periodo, int? sedeJornada)
        {
            var parameters = new Dictionary<string, object>()
            {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gra_consecutivoP", grado },
                { "per_consecutivoP", periodo },
                { "sedjor_consecutivoP", sedeJornada }
            };

            return institucion.BaseDatos.ExecuteQuery<GrupoPendienteViewModel>("pa_aca_grupos_sin_planes_academicos_retornar", parameters);
        }

        #endregion

        #region Requisitos Matrícula

        public IList<RequisitoMatriculaViewModel> ObtenerRequisitosMatricula(string texto, bool? estado, int? categoriaDocumento, string programa = null)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP",institucion.aplent_codigoP },
                { "filtros_texto", texto },
                { "reqmat_estado", estado },
                { "reqmat_doccat_consecutivo", categoriaDocumento },
                { "pro_codigoP", programa}
            };

            return institucion.BaseDatos.ExecuteQuery<RequisitoMatriculaViewModel>("pa_aca_requisitos_matricula_retornar", parametros);
        }

        public RequisitoMatriculaViewModel ObtenerDetalleRequisitoMatricula(int consecutivo)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP",institucion.aplent_codigoP },
                { "reqmat_consecutivoP", consecutivo}
            };

            return institucion.BaseDatos.ExecuteQuery<RequisitoMatriculaViewModel>("pa_aca_requisitos_matricula_detalle_retornar", parametros).FirstOrDefault();
        }

        public void IngresarRequisitoMatricula(int? categoriaDocumento, string nombre, bool adjunto, bool estado, string programas)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP",institucion.aplent_codigoP },
                { "reqmat_nombre", nombre },
                { "reqmat_requiere_adjunto", adjunto },
                { "reqmat_estado", estado },
                { "reqmat_doccat_consecutivo", categoriaDocumento },
                { "reqmat_programas", programas}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_requisitos_matricula_ingresar", parametros);
        }

        public void ModificarRequisitoMatricula(int consecutivo, int? categoriaDocumento, string nombre, bool adjunto, bool estado, string programas)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP",institucion.aplent_codigoP },
                { "reqmat_consecutivoP", consecutivo },
                { "reqmat_nombre", nombre },
                { "reqmat_requiere_adjunto", adjunto },
                { "reqmat_estado", estado },
                { "reqmat_doccat_consecutivo", categoriaDocumento },
                { "reqmat_programas", programas}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_requisitos_matricula_modificar", parametros);
        }

        public void EliminarRequisitoMatricula(int consecutivo)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP",institucion.aplent_codigoP },
                { "reqmat_consecutivoP", consecutivo}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_requisitos_matricula_eliminar", parametros);
        }

        public IList<RequisitoMatriculaEstudianteViewModel> ObtenerRequisitosMatriculaEstudiante(string codigo, int? inscripcion = null)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP",institucion.aplent_codigoP },
                { "estreqmat_per_codigo", codigo },
                { "estreqmat_ins_consecutivo", inscripcion }
            };

            return institucion.BaseDatos.ExecuteQuery<RequisitoMatriculaEstudianteViewModel>("pa_per_estudiantes_requisitos_matricula_retornar", parametros);
        }

        /// <summary>
        /// Método basado en "ObtenerRequisitosMatriculaEstudiante" que retorna los requisitos de matricula del estudiante 
        /// donde ya se haya diligenciado el documento que exige el requisito
        /// </summary>
        /// <param name="codigo"></param>
        /// <returns></returns>
        public IList<RequisitoMatriculaEstudianteViewModel> ObtenerRequisitosMatriculaEstudianteDocumentosCompletados(string codigo)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP",institucion.aplent_codigoP },
                { "estreqmat_per_codigo", codigo },
            };

            return institucion.BaseDatos.ExecuteQuery<RequisitoMatriculaEstudianteViewModel>("pa_per_estudiantes_requisitos_matricula_inscripcion_retornar", parametros);
        }

        public void IngresarRequisitoMatriculaEstudiante(int consecutivoRequisito, string codigo, int? consecutivoDocumento, int? inscripcion = null)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP",institucion.aplent_codigoP },
                { "estreqmat_reqmat_consecutivo", consecutivoRequisito },
                { "estreqmat_per_codigo", codigo },
                { "estreqmat_doc_consecutivo", consecutivoDocumento },
                { "estreqmat_codigo_usuario", per_codigoP },
                { "aud_ip", ClientIpAddress },
                { "aud_cat_codigo", CategoriaAuditoria },
                { "aud_usuario_q10", codigo_ejecutivo_institucion },
                { "aud_interno", false },
                { "estreqmat_ins_consecutivo", inscripcion }
            };

            Logger.Instance.Debug($"consecutivoRequisito:{consecutivoRequisito}, codigo:{codigo}, " +
                $"consecutivoDocumento:{consecutivoDocumento}, inscripcion:{inscripcion}, StackTrace: {Environment.StackTrace}", "REQUISITOS_MATRICULA");

            institucion.BaseDatos.ExecuteNonQuery("pa_per_estudiantes_requisitos_matricula_ingresar", parametros);
        }

        public void EliminarRequisitoMatriculaEstudiante(int consecutivoRequisito, string codigo, int? inscripcion = null)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP",institucion.aplent_codigoP },
                { "estreqmat_reqmat_consecutivo", consecutivoRequisito },
                { "estreqmat_per_codigo", codigo },
                { "aud_usuario_sesion", per_codigoP },
                { "aud_ip", ClientIpAddress },
                { "aud_cat_codigo", CategoriaAuditoria },
                { "aud_usuario_q10", codigo_ejecutivo_institucion },
                { "aud_interno", false },
                { "estreqmat_ins_consecutivo", inscripcion }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_per_estudiantes_requisitos_matricula_eliminar", parametros);
        }

        public bool ValidarRequisitoMatricula(string codigo, int? inscripcion = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "estreqmat_per_codigo", codigo },
                { "estreqmat_ins_consecutivo", inscripcion }
            };

            return Convert.ToBoolean(institucion.BaseDatos.ExecuteNonQuery("pa_per_estudiantes_requisitos_matricula_validar", parametros));
        }

        public bool ValidarRequisitoMatriculaProgramas(int? consecutivoRequisitoMatricula = null, int? consecutivoCategoriaDocumento = null, string codigosProgramas = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "reqmat_consecutivoP", consecutivoRequisitoMatricula },
                { "reqmat_doccat_consecutivo", consecutivoCategoriaDocumento },
                { "reqmat_programas", codigosProgramas}
            };

            return Convert.ToBoolean(institucion.BaseDatos.ExecuteNonQuery("pa_aca_requisitos_matricula_validar", parametros));
        }

        #endregion Requisitos Matrícula

        #region Requisitos Vuelo

        public IList<RequisitoVueloViewModel> ObtenerRequisitosVuelo(string texto, bool? estado, int? categoriaDocumento, string programa = null)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP",institucion.aplent_codigoP },
                { "filtros_texto", texto },
                { "reqvue_estado", estado },
                { "reqvue_doccat_consecutivo", categoriaDocumento },
                { "pro_codigoP", programa}
            };

            return institucion.BaseDatos.ExecuteQuery<RequisitoVueloViewModel>("pa_aca_requisitos_vuelo_retornar", parametros);
        }

        public RequisitoVueloViewModel ObtenerDetalleRequisitoVuelo(int consecutivo)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP",institucion.aplent_codigoP },
                { "reqvue_consecutivoP", consecutivo}
            };

            return institucion.BaseDatos.ExecuteQuery<RequisitoVueloViewModel>("pa_aca_requisitos_vuelo_detalle_retornar", parametros).FirstOrDefault();
        }

        public void IngresarRequisitoVuelo(int? categoriaDocumento, string nombre, bool adjunto, bool estado, string programas, bool fechaVencimiento)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP",institucion.aplent_codigoP },
                { "reqvue_nombre", nombre },
                { "reqvue_requiere_adjunto", adjunto },
                { "reqvue_estado", estado },
                { "reqvue_doccat_consecutivo", categoriaDocumento },
                { "reqvue_programas", programas},
                { "reqvue_requiere_fecha_fencimiento", fechaVencimiento}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_requisitos_vuelo_ingresar", parametros);
        }

        public void ModificarRequisitoVuelo(int consecutivo, int? categoriaDocumento, string nombre, bool adjunto, bool estado, string programas, bool fechaVencimiento)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP",institucion.aplent_codigoP },
                { "reqvue_consecutivoP", consecutivo },
                { "reqvue_nombre", nombre },
                { "reqvue_requiere_adjunto", adjunto },
                { "reqvue_estado", estado },
                { "reqvue_doccat_consecutivo", categoriaDocumento },
                { "reqvue_programas", programas},
                { "reqvue_requiere_fecha_fencimiento", fechaVencimiento}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_requisitos_vuelo_modificar", parametros);
        }

        public void EliminarRequisitoVuelo(int consecutivo)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP",institucion.aplent_codigoP },
                { "reqvue_consecutivoP", consecutivo}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_requisitos_vuelo_eliminar", parametros);
        }

        public bool ValidarRelacionCategoriasProgramas(string programas, int? categoria, int? requisito)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP",institucion.aplent_codigoP },
                { "reqvue_consecutivoP", requisito},
                { "reqvue_doccat_consecutivo", categoria},
                { "programas", programas }
            };

            return Convert.ToBoolean(institucion.BaseDatos.ExecuteNonQuery("pa_par_documentos_categorias_relacionados_validar", parametros));
        }

        #region Requisitos Vuelo - Estudiante

        public IList<RequisitoVueloEstudianteViewModel> ObtenerRequisitosVueloEstudiante(string codigo)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP",institucion.aplent_codigoP },
                { "estreqvue_per_codigo", codigo }
            };

            return institucion.BaseDatos.ExecuteQuery<RequisitoVueloEstudianteViewModel>("pa_per_estudiantes_requisitos_vuelo_retornar", parametros);
        }

        public RequisitoVueloEstudianteViewModel ObtenerDetalleRequisitoVueloEstudiante(int consecutivo)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP",institucion.aplent_codigoP },
                { "estreqvue_consecutivoP", consecutivo }
            };

            return institucion.BaseDatos.ExecuteQuery<RequisitoVueloEstudianteViewModel>("pa_per_estudiantes_requisitos_vuelo_detalle_retornar", parametros).FirstOrDefault();
        }

        public void IngresarRequisitoVueloEstudiante(int consecutivoRequisito, string codigo, int? consecutivoDocumento, DateTime? fechaVencimiento)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP",institucion.aplent_codigoP },
                { "estreqvue_reqvue_consecutivo", consecutivoRequisito },
                { "estreqvue_per_codigo", codigo },
                { "estreqvue_doc_consecutivo", consecutivoDocumento },
                { "estreqvue_fec_vencimiento", fechaVencimiento },
                { "estreqvue_codigo_usuario", per_codigoP },
                { "aud_ip", ClientIpAddress },
                { "aud_cat_codigo", CategoriaAuditoria },
                { "aud_usuario_q10", codigo_ejecutivo_institucion },
                { "aud_interno", false }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_per_estudiantes_requisitos_vuelo_ingresar", parametros);
        }

        public void ActualizarRequisitoVueloEstudiante(int consecutivo, DateTime? fechaVencimiento)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP",institucion.aplent_codigoP },
                { "estreqvue_consecutivoP", consecutivo },
                { "estreqvue_fec_vencimiento", fechaVencimiento }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_per_estudiantes_requisitos_vuelo_modificar", parametros);
        }

        public void EliminarRequisitoVueloEstudiante(int consecutivoRequisito, string codigo)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP",institucion.aplent_codigoP },
                { "estreqvue_reqvue_consecutivo", consecutivoRequisito },
                { "estreqvue_per_codigo", codigo },
                { "aud_usuario_sesion", per_codigoP },
                { "aud_ip", ClientIpAddress },
                { "aud_cat_codigo", CategoriaAuditoria },
                { "aud_usuario_q10", codigo_ejecutivo_institucion },
                { "aud_interno", false }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_per_estudiantes_requisitos_vuelo_eliminar", parametros);
        }

        public bool ValidarRequisitoVuelo(string codigo, string programa)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "estreqvue_per_codigo", codigo },
                { "pro_codigoP", programa }
            };

            return Convert.ToBoolean(institucion.BaseDatos.ExecuteNonQuery("pa_per_estudiantes_requisitos_vuelo_validar", parametros));
        }

        #endregion Requisitos Vuelo - Estudiante

        #endregion Requisitos Vuelo

        #region Requisitos Grados

        public IList<RequisitoGradoViewModel> ObtenerRequisitosGrado(string texto, bool? estado, int? categoriaDocumento, string programa = null, int? grado = null)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP",institucion.aplent_codigoP },
                { "filtros_texto", texto },
                { "reqgra_estado", estado },
                { "pro_codigoP", programa },
                { "gra_consecutivoP", grado },
                { "reqgra_doccat_consecutivo", categoriaDocumento}
            };

            return institucion.BaseDatos.ExecuteQuery<RequisitoGradoViewModel>("pa_aca_requisitos_grados_retornar", parametros);
        }

        public RequisitoGradoViewModel ObtenerDetalleRequisitoGrado(int consecutivo)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP",institucion.aplent_codigoP },
                { "reqgra_consecutivoP", consecutivo}
            };

            return institucion.BaseDatos.ExecuteQuery<RequisitoGradoViewModel>("pa_aca_requisitos_grados_detalle_retornar", parametros).FirstOrDefault();
        }

        public void IngresarRequisitoGrado(int? categoriaDocumento, string nombre, bool adjunto, bool estado, string programas = null, string grados = null)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP",institucion.aplent_codigoP },
                { "reqgra_nombre", nombre },
                { "reqgra_requiere_adjunto", adjunto },
                { "reqgra_estado", estado },
                { "reqgra_programas", programas },
                { "reqgra_grado", grados },
                { "reqgra_doccat_consecutivo", categoriaDocumento}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_requisitos_grados_ingresar", parametros);
        }

        public void ModificarRequisitoGrado(int consecutivo, int? categoriaDocumento, string nombre, bool adjunto, bool estado, string programas = null, string grados = null)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP",institucion.aplent_codigoP },
                { "reqgra_consecutivoP", consecutivo },
                { "reqgra_nombre", nombre },
                { "reqgra_requiere_adjunto", adjunto },
                { "reqgra_estado", estado },
                { "reqgra_programas", programas },
                { "reqgra_grado", grados },
                { "reqgra_doccat_consecutivo", categoriaDocumento}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_requisitos_grados_modificar", parametros);
        }

        public void EliminarRequisitoGrado(int consecutivo)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP",institucion.aplent_codigoP },
                { "reqgra_consecutivoP", consecutivo}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_requisitos_grados_eliminar", parametros);
        }

        public IList<RequisitoGradoEstudianteViewModel> ObtenerRequisitosGradoEstudiante(string codigo, int? inscripcion = null)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP",institucion.aplent_codigoP },
                { "estreqgra_per_codigo", codigo },
                { "estreqgra_ins_consecutivo", inscripcion}
            };

            return institucion.BaseDatos.ExecuteQuery<RequisitoGradoEstudianteViewModel>("pa_per_estudiantes_requisitos_grados_retornar", parametros);
        }

        public void IngresarRequisitoGradoEstudiante(int consecutivoRequisito, string codigo, int? consecutivoDocumento, int? inscripcion = null)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP",institucion.aplent_codigoP },
                { "estreqgra_reqgra_consecutivo", consecutivoRequisito },
                { "estreqgra_per_codigo", codigo },
                { "estreqgra_doc_consecutivo", consecutivoDocumento },
                { "estreqgra_ins_consecutivo", inscripcion },
                { "estreqgra_codigo_usuario", per_codigoP },
                { "aud_ip",ClientIpAddress },
                { "aud_cat_codigo", CategoriaAuditoria },
                { "aud_usuario_q10", codigo_ejecutivo_institucion },
                { "aud_interno", false }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_per_estudiantes_requisitos_grados_ingresar", parametros);
        }

        public void EliminarRequisitoGradoEstudiante(int consecutivoRequisito, string codigo, int? inscripcion = null)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP",institucion.aplent_codigoP },
                { "estreqgra_reqgra_consecutivo", consecutivoRequisito },
                { "estreqgra_per_codigo", codigo },
                { "estreqgra_ins_consecutivo", inscripcion },
                { "aud_usuario_sesion", per_codigoP },
                { "aud_ip",ClientIpAddress },
                { "aud_cat_codigo", CategoriaAuditoria },
                { "aud_usuario_q10", codigo_ejecutivo_institucion },
                { "aud_interno", false }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_per_estudiantes_requisitos_grados_eliminar", parametros);
        }

        public bool ValidarRequisitoGrado(string codigo, int? inscripcion = null, string programa = null, int? grado = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "estreqgra_per_codigo", codigo },
                { "estreqgra_ins_consecutivo", inscripcion },
                { "estreqgra_pro_codigo", programa },
                { "estreqgra_gra_consecutivo", grado }
            };

            return Convert.ToBoolean(institucion.BaseDatos.ExecuteNonQuery("pa_per_estudiantes_requisitos_grados_validar", parametros));
        }

        public bool ValidarExisteRequisitoGrado(string programas, int? requisito = null, int? categoriaDocumento = null, string grados = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "programas", programas },
                { "reqgra_consecutivoP", requisito },
                { "reqgra_doccat_consecutivo", categoriaDocumento },
                { "grados", grados}
            };

            return Convert.ToBoolean(institucion.BaseDatos.ExecuteNonQuery("pa_aca_requisitos_grados_validar", parametros));
        }

        #endregion

        #region Planes Academicos

        public IList<AsignaturaViewModel> ObtenerAsignaturasParaAsignarPlanes(string area, int consecutivoGrupo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "asi_are_codigo", area },
                { "plaaca_gru_consecutivo", consecutivoGrupo }
            };

            return institucion.BaseDatos.ExecuteQuery<AsignaturaViewModel>("pa_aca_asignaturas_para_asignar_areas_retornar", parametros);
        }

        public IList<PlanAcademicoViewModel> ObtenerPlanesAcademicosColegioMovil(string docente = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "per_codigoP", docente }
            };

            return institucion.BaseDatos.ExecuteQuery<PlanAcademicoViewModel>("pa_aca_planes_academicos_activos_colegios_retornar", parametros);
        }

        public IList<PlanAcademicoViewModel> ObtenerPlanesAcademicosColegio(int? consecutivoGrupo, string texto = null, int? consecutivoGrado = null, string asignatura = null, int? periodo = null, int? sedejornada = null, string docente = null, bool? sinGrupo = false, int? sedejornadaGrado = null, char? filtro = null, bool? tradicionales = null, bool esDirectorGrupo = false)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaaca_gru_consecutivo", consecutivoGrupo },
                { "gra_consecutivoP", consecutivoGrado },
                { "asi_codigoP", asignatura },
                { "per_consecutivoP", periodo },
                { "sedjor_consecutivoP", sedejornada },
                { "sedjorprogra_consecutivoP", sedejornadaGrado },
                { "plaaca_per_codigo_docente", docente },
                { "filtro_texto", texto },
                { "filtro_sin_grupo", sinGrupo },
                { "per_codigoP", esDirectorGrupo ? null : per_codigoP },
                { "rol_codigoP", rol_codigoP },
                { "filtro_tipo", filtro },
                { "filtro_tradicionales", tradicionales}
            };

            return institucion.BaseDatos.ExecuteQuery<PlanAcademicoViewModel>("pa_aca_planes_academicos_colegios_retornar", parametros);
        }       

        public async Task<IPagedList<PlanAcademicoViewModel>> ObtenerCursosRotativosApiAsync(int? consecutivoSedeJornada, int? consecutivoAnnoLectivo, int? consecutivoGrado, string codigoAsignatura, string codigoDocente, int pagina = 1, int tamanioPagina = Constantes.PAGINACION_TAMANO_PAGINA)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_CodigoP", institucion.aplent_codigoP },
                { "plaaca_per_consecutivo", consecutivoAnnoLectivo },
                { "plaaca_asi_codigo", codigoAsignatura },
                { "sedjor_consecutivoP", consecutivoSedeJornada },
                { "gra_consecutivoP", consecutivoGrado },
                { "plaaca_per_codigo_docente", codigoDocente }
            };

            var (listaCursosRotatuvos, cantidadTotalCursosRotativos) = await institucion.BaseDatos.ExecuteQueryAsync<PlanAcademicoViewModel>("pa_apis_aca_planes_academicos_rotativos_retornar", parametros, pagina, tamanioPagina);
            return new StaticPagedList<PlanAcademicoViewModel>(listaCursosRotatuvos, pagina, tamanioPagina, cantidadTotalCursosRotativos);
        }

        public PlanAcademicoViewModel ObtenerDetallePlanAcademicoColegio(int consecutivoPlan)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaaca_consecutivoP", consecutivoPlan }
            };

            return institucion.BaseDatos.ExecuteQuery<PlanAcademicoViewModel>("pa_aca_planes_academicos_colegios_detalle_retornar", parametros).FirstOrDefault();
        }

        public int IngresarPlanAcademicoColegio(int? consecutivoGrupo, string asignatura, string especialidad, string area, string docente, int? grado, int? periodo, decimal? intensidadHoraria, decimal? porcentaje, bool rotativo, string nombre = null, DateTime? fechaInicio = null, DateTime? fechaFin = null, char? origenAuditoria = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaaca_gru_consecutivo", consecutivoGrupo },
                { "plaaca_asi_codigo", asignatura },
                { "plaaca_esp_codigo", especialidad },
                { "plaaca_are_codigo", area },
                { "plaaca_sedjorprogra_consecutivo", grado },
                { "plaaca_per_consecutivo", periodo },
                { "plaaca_per_codigo_docente", docente },
                { "plaaca_intensidad_horario_semanal", intensidadHoraria },
                { "plaaca_porcentaje", porcentaje },
                { "plaaca_rotativo", rotativo },
                { "plaaca_nombre", nombre },
                { "plaaca_fecha_inicio", fechaInicio },
                { "plaaca_fecha_fin", fechaFin },
                { "aud_usuario_sesion", per_codigoP },
                { "aud_ip", ClientIpAddress },
                { "aud_cat_codigo", CategoriaAuditoria },
                { "aud_usuario_q10", codigo_ejecutivo_institucion },
                { "aud_origen", origenAuditoria },
                { "aud_interno", false }
            };

            var result = institucion.BaseDatos.ExecuteNonQuery("pa_aca_planes_academicos_colegios_ingresar", parametros);

            AsyncHelpers.RunSync(() => new ServicioSuggestions(institucion, usuario).ActualizarAccesoDocentesAdministrativos(docente, "D", null)); // "D" -> significa docente

            return result;
        }

        public async Task<int> IngresarPlanAcademicoColegioAsync(int? consecutivoGrupo, string asignatura, string especialidad, string area, string docente, int? grado, int? periodo, decimal? intensidadHoraria, decimal? porcentaje, bool rotativo, string nombre = null, DateTime? fechaInicio = null, DateTime? fechaFin = null, char? origenAuditoria = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaaca_gru_consecutivo", consecutivoGrupo },
                { "plaaca_asi_codigo", asignatura },
                { "plaaca_esp_codigo", especialidad },
                { "plaaca_are_codigo", area },
                { "plaaca_sedjorprogra_consecutivo", grado },
                { "plaaca_per_consecutivo", periodo },
                { "plaaca_per_codigo_docente", docente },
                { "plaaca_intensidad_horario_semanal", intensidadHoraria },
                { "plaaca_porcentaje", porcentaje },
                { "plaaca_rotativo", rotativo },
                { "plaaca_nombre", nombre },
                { "plaaca_fecha_inicio", fechaInicio },
                { "plaaca_fecha_fin", fechaFin },
                { "aud_usuario_sesion", per_codigoP },
                { "aud_ip", ClientIpAddress },
                { "aud_cat_codigo", CategoriaAuditoria },
                { "aud_usuario_q10", codigo_ejecutivo_institucion },
                { "aud_origen", origenAuditoria },
                { "aud_interno", false }
            };

            var result = await institucion.BaseDatos.ExecuteNonQueryAsync("pa_aca_planes_academicos_colegios_ingresar", parametros);

            await new ServicioSuggestions(institucion, usuario).ActualizarAccesoDocentesAdministrativos(docente, "D", null); // "D" -> significa docente

            return result;
        }

        public void ModificarPlanAcademicoColegio(int consecutivoPlan, string especialidad, string docente, decimal? intensidadHoraria, decimal? porcentaje, string nombre = null, string asignatura = null, int? grado = null, int? periodo = null, DateTime? fechaInicio = null, DateTime? fechaFin = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaaca_consecutivoP", consecutivoPlan },
                { "plaaca_esp_codigo", especialidad },
                { "plaaca_per_codigo_docente", docente },
                { "plaaca_intensidad_horario_semanal", intensidadHoraria },
                { "plaaca_porcentaje", porcentaje },
                { "plaaca_nombre", nombre },
                { "plaaca_asi_codigo", asignatura },
                { "plaaca_sedjorprogra_consecutivo", grado },
                { "plaaca_per_consecutivo", periodo },
                { "plaaca_fecha_inicio", fechaInicio },
                { "plaaca_fecha_fin", fechaFin}
            };

            var estudiantes = AsyncHelpers.RunSync(() => new ServicioSuggestions(institucion, usuario).ObtenerAccesoPersonas(docente, "D"));

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_planes_academicos_colegios_modificar", parametros);

            AsyncHelpers.RunSync(() => new ServicioSuggestions(institucion, usuario).ActualizarAccesoDocentesAdministrativos(docente, "D", null, estudiantes));
        }

        public void ModificarNumeroReprobacion(int areaReprobacion, int numero)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "arerep_consecutivoP", areaReprobacion },
                { "arerep_numero_asi_repro", numero }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_areas_reprobaciones_modificar", parametros);
        }

        public void ImportarPlanesAcademico(int? origen, int? destino, char? filtro = null, string planesimportar = null)
        {
            var parameters = new Dictionary<string, object>(){
                { "aplent_codigoP" , institucion.aplent_codigoP },
                { "gru_consecutivo_origen" , origen },
                { "gru_consecutivo_destino" , destino },
                { "filtro_tipo " , filtro },
                { "gru_plaaca_consecutivo" , planesimportar }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_grupos_planes_academicos_copiar", parameters);

        }

        public IList<ClonarPlanAcademicoViewModel> ObtenerExcepcionesClonacionColegios(int plaaca_consecutivoP_origen, string asignatura, int grado, int periodo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaaca_consecutivoP_origen", plaaca_consecutivoP_origen },
                { "asi_codigoP", asignatura },
                { "sedjorprogra_consecutivoP", grado },
                { "per_consecutivoP", periodo },
            };

            return institucion.BaseDatos.ExecuteQuery<ClonarPlanAcademicoViewModel>("pa_aca_planes_academicos_colegio_clonar_estudiantes_validar", parametros);
        }

        #region Periodos - Area Grupo

        public IList<PeriodoGradoViewModel> ObtenerPeriodosAreaGrupo(string area, int? grupo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gru_consecutivoP ", grupo },
                { "are_codigoP ", area }
            };

            return institucion.BaseDatos.ExecuteQuery<PeriodoGradoViewModel>("pa_aca_area_grados_periodos_parametros_retornar", parametros);
        }

        #endregion Periodos - Area Grupo

        #endregion Planes Academicos

        #region Docentes de apoyo

        public bool ValidarDocenteApoyoCurso(int consecutivoCurso, string codigoDocente)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaaca_consecutivoP", consecutivoCurso },
                { "per_codigoP", codigoDocente }
            };

            return Convert.ToBoolean(institucion.BaseDatos.ExecuteNonQuery("pa_aca_planes_academicos_docentes_apoyo_validar", parametros));
        }

        public List<DocenteApoyoViewModel> ObtenerDocentesApoyo(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaaca_consecutivos", consecutivo.ToString() }, // Se convierte a string ya que el SP recibe una cadena de consecutivos
            };

            return institucion.BaseDatos.ExecuteQuery<DocenteApoyoViewModel>("pa_aca_planes_academicos_docentes_retornar", parametros).ToList();
        }

        public async Task<IList<DocenteApoyoViewModel>> ObtenerDocentesApoyoAsync(string consecutivosPlanesAcademicos = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaaca_consecutivos", consecutivosPlanesAcademicos },
            };

            return await institucion.BaseDatos.ExecuteQueryAsync<DocenteApoyoViewModel>("pa_aca_planes_academicos_docentes_retornar", parametros);
        }

        public int IngresarDocenteApoyo(int planAcademico, string codigoDocente)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaacadoc_plaaca_consecutivo", planAcademico },
                { "plaacadoc_per_codigo", codigoDocente },
                { "aud_ip",ClientIpAddress },
                { "aud_cat_codigo", CategoriaAuditoria },
                { "aud_usuario_q10", codigo_ejecutivo_institucion },
                { "aud_interno", false },
                { "aud_usuario_sesion", per_codigoP }
            };

            var result = institucion.BaseDatos.ExecuteNonQuery("pa_aca_planes_academicos_docentes_ingresar", parametros);

            AsyncHelpers.RunSync(() => new ServicioSuggestions(institucion, usuario).ActualizarAccesoDocentesAdministrativos(codigoDocente, "D", null));

            return result;
        }

        public int AsignarDocenteTitular(int planAcademico, string codigoDocente)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaaca_consecutivoP", planAcademico },
                { "per_codigoP", codigoDocente },
                { "aud_ip",ClientIpAddress },
                { "aud_cat_codigo", CategoriaAuditoria },
                { "aud_usuario_q10", codigo_ejecutivo_institucion },
                { "aud_interno", false },
                { "aud_usuario_sesion", per_codigoP }
            };

            var estudiantes = AsyncHelpers.RunSync(() => new ServicioSuggestions(institucion, usuario).ObtenerAccesoPersonas(codigoDocente, "D"));

            var result = institucion.BaseDatos.ExecuteNonQuery("pa_aca_planes_academicos_docentes_titular_asignar", parametros);

            AsyncHelpers.RunSync(() => new ServicioSuggestions(institucion, usuario).ActualizarAccesoDocentesAdministrativos(codigoDocente, "D", null, estudiantes));

            return result;
        }

        public void EliminarDocenteApoyo(int consecutivo, string codigoDocente)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaacadoc_consecutivoP", consecutivo},
                { "aud_ip",ClientIpAddress },
                { "aud_cat_codigo", CategoriaAuditoria },
                { "aud_usuario_q10", codigo_ejecutivo_institucion },
                { "aud_interno", false },
                { "aud_usuario_sesion", per_codigoP }
            };

            var estudiantes = AsyncHelpers.RunSync(() => new ServicioSuggestions(institucion, usuario).ObtenerAccesoPersonas(codigoDocente, "D"));

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_planes_academicos_docentes_eliminar", parametros);

            AsyncHelpers.RunSync(() => new ServicioSuggestions(institucion, usuario).ActualizarAccesoDocentesAdministrativos(codigoDocente, "D", null, estudiantes));

        }

        #endregion Docentes de apoyo

        #region  Docentes de reemplazo

        public List<DocenteReemplazoViewModel> ObtenerDocentesReemplazo(int planAcademico, DateTime? fechaInicio = null, DateTime? fechaFin = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaacadocree_plaaca_consecutivo", planAcademico },
                { "plaacadocree_fecha_habilitacion_curso", fechaInicio },
                { "plaacadocree_fecha_cierre_curso", fechaFin }
            };

            return institucion.BaseDatos.ExecuteQuery<DocenteReemplazoViewModel>("paz_comfama_aca_planes_academicos_docentes_reemplazo_retornar", parametros).ToList();
        }

        public DocenteReemplazoViewModel ObtenerInformacionDocenteReemplazo(int planAcademico, DateTime? fechaInicio = null, DateTime? fechaFin = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaacadocree_plaaca_consecutivo", planAcademico },
                { "plaacadocree_fecha_habilitacion_curso", fechaInicio },
                { "plaacadocree_fecha_cierre_curso", fechaFin }
            };

            return institucion.BaseDatos.ExecuteQuery<DocenteReemplazoViewModel>("paz_comfama_aca_planes_academicos_docentes_reemplazo_info_retornar", parametros).FirstOrDefault();
        }

        public DocenteReemplazoViewModel ObtenerDetalleDocenteReemplazo(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaacadocree_consecutivoP", consecutivo }
            };

            return institucion.BaseDatos.ExecuteQuery<DocenteReemplazoViewModel>("paz_comfama_aca_planes_academicos_docentes_reemplazo_detalle_retornar", parametros).FirstOrDefault();
        }

        public int IngresarDocenteReemplazo(string codigoDocente, int planAcademico, DateTime? fecha_inicio, DateTime? fecha_fin, DateTime? fecha_habilitacion_curso, DateTime? fecha_cierre_curso, string motivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaacadocree_per_codigo", codigoDocente },
                { "plaacadocree_plaaca_consecutivo", planAcademico },
                { "plaacadocree_fecha_inicio", fecha_inicio },
                { "plaacadocree_fecha_fin", fecha_fin },
                { "plaacadocree_fecha_habilitacion_curso", fecha_habilitacion_curso },
                { "plaacadocree_fecha_cierre_curso", fecha_cierre_curso },
                { "plaacadocree_motivo", motivo }
            };

            var result = institucion.BaseDatos.ExecuteNonQuery("paz_comfama_aca_planes_academicos_docentes_reemplazo_ingresar", parametros);
            return result;
        }

        public void ModificarDocenteReemplazo(int consecutivo, DateTime? fecha_inicio, DateTime? fecha_fin, DateTime? fecha_habilitacion_curso, DateTime? fecha_cierre_curso, string motivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaacadocree_consecutivoP", consecutivo },
                //{ "plaacadocree_per_codigo", codigoDocente },
                { "plaacadocree_fecha_inicio", fecha_inicio },
                { "plaacadocree_fecha_fin", fecha_fin },
                { "plaacadocree_fecha_habilitacion_curso", fecha_habilitacion_curso },
                { "plaacadocree_fecha_cierre_curso", fecha_cierre_curso },
                { "plaacadocree_motivo", motivo }
            };

            institucion.BaseDatos.ExecuteNonQuery("paz_comfama_aca_planes_academicos_docentes_reemplazo_modificar", parametros);
        }

        public void EliminarDocenteReemplazo(int consecutivo_plaaca)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaacadocree_consecutivoP", consecutivo_plaaca }
            };

            institucion.BaseDatos.ExecuteNonQuery("paz_comfama_aca_planes_academicos_docentes_reemplazo_eliminar", parametros);

        }

        #region Cruce Horario Docentes

        public bool ValidarCruceHorarioDocente(string docente, int? consecutivo_plaaca)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaaca_per_codigo_docente", docente },
                { "plaaca_consecutivoP", consecutivo_plaaca }
            };

            return Convert.ToBoolean(institucion.BaseDatos.ExecuteNonQuery("paz_comfama_aca_horarios_conflicto_por_repeticion_colegios_revisar", parametros));
        }

        #endregion Cruce Horario Docentes

        #endregion Docentes de reemplazo

        #region Asignaturas Semestrales

        public IList<PeriodoPorcentajeViewModel> ObtenerPeriodosPlanAcademico(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaacagraper_plaaca_consecutivo", consecutivo }
            };

            return institucion.BaseDatos.ExecuteQuery<PeriodoPorcentajeViewModel>("pa_aca_planes_academicos_grados_periodos_retornar", parametros);
        }

        public int IngresarPeriodoPlanAcademico(int planAcademico, int gradoPeriodo, decimal? porcentaje = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaacagraper_plaaca_consecutivo", planAcademico },
                { "plaacagraper_graper_consecutivo", gradoPeriodo },
                { "plaacagraper_porcentaje", porcentaje }
            };

            return institucion.BaseDatos.ExecuteNonQuery("pa_aca_planes_academicos_grados_periodos_ingresar", parametros);
        }

        public async void IngresarPeriodosMasivoPlanAcademicoAsync(int planAcademico, string gradosPeriodos)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaacagraper_plaaca_consecutivo", planAcademico },
                { "plaacagraper_graper_consecutivos", gradosPeriodos }
            };

            await institucion.BaseDatos.ExecuteNonQueryAsync("pa_aca_planes_academicos_grados_periodos_masivo_ingresar", parametros);
        }

        public int ModificarPeriodoPlanAcademico(int? plangradoPeriodo, decimal? porcentaje = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaacagraper_consecutivoP", plangradoPeriodo },
                { "plaacagraper_porcentaje", porcentaje },
                { "aud_ip",ClientIpAddress },
                { "aud_cat_codigo", CategoriaAuditoria },
                { "aud_usuario_q10", codigo_ejecutivo_institucion },
                { "aud_interno", false },
                { "aud_usuario_sesion", per_codigoP }
            };

            return institucion.BaseDatos.ExecuteNonQuery("pa_aca_planes_academicos_grados_periodos_porcentaje_modificar", parametros);
        }

        public void EliminarPeriodoPlanAcademico(int planAcademico)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaacagraper_plaaca_consecutivo", planAcademico }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_planes_academicos_grados_periodos_eliminar", parametros);
        }

        public void EliminarPeriodoPlanAcademicoGrupo(int plangradoPeriodo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaacagraper_consecutivoP", plangradoPeriodo },
                { "aud_ip",ClientIpAddress },
                { "aud_cat_codigo", CategoriaAuditoria },
                { "aud_usuario_q10", codigo_ejecutivo_institucion },
                { "aud_interno", false },
                { "aud_usuario_sesion", per_codigoP }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_planes_academicos_grados_periodo_eliminar", parametros);
        }

        public bool ValidarPeriodoPlanAcademicoGrupoEliminable(int gradoPeriodo, int planAcademico)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "graper_consecutivoP", gradoPeriodo },
                { "plaaca_consecutivoP", planAcademico}
            };

            return Convert.ToBoolean(institucion.BaseDatos.ExecuteNonQuery("pa_aca_grados_periodos_notas_validar", parametros));
        }

        #endregion

        #region Grupos Trabajo - Planes académicos

        public IList<GrupoTrabajoViewModel> ObtenerPlanesAcademicosSuperior(string texto, string docente, int? periodo, int? sedeJornada, int? PensumPrograma, string asignatura, DateTime? fechaInicio, DateTime? fechaFin, string programa = null, bool? estado = false, string modulo = null, char? filtro = 'A', int? cantidadMinima = null, bool? incluir_correo_enviado = null, string variablePersonalizada = null, string sede = null, bool? cerrados = null, int? modalidad = null, char? tipoEvaluacion = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "per_codigoP", per_codigoP },
                { "filtros_docente", docente },
                { "filtros_periodo", periodo },
                { "filtros_sedejornada", sedeJornada },
                { "tipo_evaluacion", tipoEvaluacion},
                //Filtro por sede para el colombo
                { "filtro_sede", sede },
                { "filtros_programa_pensum", PensumPrograma },
                { "filtros_asignatura", asignatura },
                { "filtros_fecha_inicio", fechaInicio },
                { "filtros_fecha_fin", fechaFin },
                { "filtros_texto", texto },
                { "filtros_programa", programa },
                { "tipmod_consecutivoP", modalidad },
                { "filtros_estado", estado },//true: Archivados  false:En curso  null:todos
                { "filtros_modulo", modulo },
                { "filtrar_solo_por", filtro },
                { "cantidad_estudiantes", cantidadMinima },
                { "rol_codigoP", rol_codigoP },
                { "plaaca_correo_enviado", incluir_correo_enviado},
                //Colombo: modelo evaluacion programa
                { "pro_variable1", variablePersonalizada },
                { "filtro_cerrado", cerrados}
            };

            return institucion.BaseDatos.ExecuteQuery<GrupoTrabajoViewModel>("pa_aca_planes_academicos_trabajo_superior_retornar", parametros);
        }

        public async Task<IPagedList<GrupoTrabajoViewModel>> ObtenerPlanesAcademicosSuperiorApisAsync(char tipoCurso, char? estadoCursos, int? periodo = null, string programa = null, int? sedeJornada = null, int? curso = null, int tamanioPagina = 12, int pagina = 1)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtro_tipo_curso ", tipoCurso },
                { "filtro_estado", estadoCursos },
                { "per_consecutivoP", periodo },
                { "pro_codigoP", programa },
                { "sedjor_consecutivoP", sedeJornada },
                { "plaaca_consecutivoP", curso }
            };

            var result = await institucion.BaseDatos.ExecuteQueryAsync<GrupoTrabajoViewModel>("pa_apis_planes_academicos_retornar", parametros, pagina, tamanioPagina);
            return new StaticPagedList<GrupoTrabajoViewModel>(result.Item1, pagina, tamanioPagina, result.Item2);
        }

        public IList<GrupoTrabajoViewModel> ObtenerPlanesAcademicosSuperiorModulosPadre(int? periodo, int? sedeJornada, string modPadre, bool? estado = false)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "sedjor_consecutivoP", sedeJornada },
                { "per_consecutivoP", periodo },
                { "modpad_codigoP", modPadre },
            };

            return institucion.BaseDatos.ExecuteQuery<GrupoTrabajoViewModel>("pa_aca_planes_academicos_por_modulo_padre_retornar", parametros);
        }

        public IPagedList<GrupoTrabajoViewModel> ObtenerPlanesAcademicosSuperior(string texto, string docente, int? periodo, int? sedeJornada, int? PensumPrograma, string asignatura, DateTime? fechaInicio, DateTime? fechaFin, int pageIndex, string programa = null, bool? estado = false, string modulo = null, char? filtro = 'A', int? cantidadMinima = null, bool? incluir_correo_enviado = null, string variablePersonalizada = null, TimeSpan? horaInicio = null, TimeSpan? horaFin = null, bool? cerrados = null, int? modalidad = null, bool? aplicaPrenscripcion = null, int tamanioPagina = Constantes.PAGINACION_TAMANO_PAGINA, char? tipoEvaluacion = null)
        {
            int totalCount;
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "per_codigoP", per_codigoP },
                { "filtros_docente", docente },
                { "filtros_periodo", periodo },
                { "filtros_sedejornada", sedeJornada },
                { "filtros_programa_pensum", PensumPrograma },
                { "filtros_asignatura", asignatura },
                { "filtros_fecha_inicio", fechaInicio },
                { "filtros_fecha_fin", fechaFin },
                { "filtros_texto", texto },
                { "filtros_programa", programa },
                { "filtros_estado", estado },
                { "filtros_modulo", modulo },
                { "filtrar_solo_por", filtro },
                { "cantidad_estudiantes", cantidadMinima },
                { "tipmod_consecutivoP", modalidad },
                { "plaaca_aplica_preinscripcion", aplicaPrenscripcion },
                { "rol_codigoP", rol_codigoP },
                { "plaaca_correo_enviado", incluir_correo_enviado},
                { "tipo_evaluacion", tipoEvaluacion},
                //Colombo: modelo evaluacion programa
                { "pro_variable1", variablePersonalizada },
                { "hora_inicio", horaInicio },
                { "hora_fin", horaFin },
                { "filtro_cerrado", cerrados}
            };

            var result = institucion.BaseDatos.ExecuteQuery<GrupoTrabajoViewModel>("pa_aca_planes_academicos_trabajo_superior_retornar", parametros, pageIndex, tamanioPagina, out totalCount);
            return new StaticPagedList<GrupoTrabajoViewModel>(result, pageIndex, tamanioPagina, totalCount);
        }

        public IPagedList<GrupoTrabajoViewModel> ObtenerPlanesAcademicosOtrosProgramas(string matricula, int periodo, string asignatura, string modulo, int pageIndex, char? tipoEvaluacion = null)
        {

            bool personalizacionMultiplesMatriculas = institucion.ValidarPersonalizacion(Constantes.ZPERMITIR_ASIGNATURA_VARIAS_MATRICULA);

            int totalCount;
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "per_consecutivoP", periodo },
                { "mat_codigoP", matricula },
                { "asi_codigoP", asignatura },
                { "filtro_encurso", personalizacionMultiplesMatriculas },
                { "mod_codigoP", modulo },
                { "tipo_evaluacion", tipoEvaluacion }
            };

            var result = institucion.BaseDatos.ExecuteQuery<GrupoTrabajoViewModel>("pa_aca_planes_academicos_otros_programas_retornar", parametros, pageIndex, Constantes.PAGINACION_TAMANO_PAGINA, out totalCount);
            return new StaticPagedList<GrupoTrabajoViewModel>(result, pageIndex, 6, totalCount);
        }

        public IList<GrupoTrabajoViewModel> ObtenerPlanesAcademicosSuperiorMasivos(string sedesJornadas, string programas, string periodos, bool? archivados = false, char? tipo = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "sedjor_consecutivo", sedesJornadas },
                { "pro_codigo", programas },
                { "per_consecutivo", periodos },
                { "incluir_archivados", archivados },
                { "pro_tipo_evaluacion", tipo } //tipo de evaluación
            };

            return institucion.BaseDatos.ExecuteQuery<GrupoTrabajoViewModel>("pa_aca_planes_academicos_masivo_retonar", parametros);
        }

        public GrupoTrabajoViewModel ObtenerDetallePlanAcademicoSuperior(int? consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaaca_consecutivoP", consecutivo }
            };

            return institucion.BaseDatos.ExecuteQuery<GrupoTrabajoViewModel>("pa_aca_planes_academicos_trabajo_superior_detalle_retornar", parametros).FirstOrDefault();
        }

        public GrupoTrabajoViewModel ObtenerDetalleSimplePlanAcademicoSuperior(int? consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaaca_consecutivoP", consecutivo }
            };

            return institucion.BaseDatos.ExecuteQuery<GrupoTrabajoViewModel>("pa_aca_planes_academicos_trabajo_superior_detalle_simple_retornar", parametros).FirstOrDefault();
        }

        public async Task<GrupoTrabajoViewModel> ObtenerDetallePlanAcademicoSuperiorAsync(int? consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaaca_consecutivoP", consecutivo }
            };

            return (await institucion.BaseDatos.ExecuteQueryAsync<GrupoTrabajoViewModel>("pa_aca_planes_academicos_trabajo_superior_detalle_retornar", parametros)).FirstOrDefault();
        }

        public async Task<GrupoTrabajoViewModel> ObtenerDetallePlanAcademicoApisAsync(int consecutivoPlanAcademico)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaaca_consecutivoP", consecutivoPlanAcademico }
            };

            return (await institucion.BaseDatos.ExecuteQueryAsync<GrupoTrabajoViewModel>("pa_apis_planes_academicos_detalle_retornar", parametros)).FirstOrDefault();
        }

        public IList<GrupoTrabajoViewModel> ObtenerPlanesAcademicosSuperiorConsecutivos(string consecutivos)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaaca_consecutivos", consecutivos }
            };

            return institucion.BaseDatos.ExecuteQuery<GrupoTrabajoViewModel>("pa_aca_planes_academicos_trabajo_superior_detalle_retornar", parametros);
        }

        public int IngresarPlanAcademicoSuperior(string codigoPlan, string nombre, string docente, int? sedeJornadaPrograma, int? pensumPrograma, string asignatura, int? vigencia, int? periodo, DateTime? fechaInicio, DateTime? fechaFin, int? cupoMaximo, string modulo,
            decimal? porcentajeComision = null, string codigoVirtual = null, string centroCostos = null, string centroCosto = null, char? modalidad = null, string infCodigo = null, string optativa = null, int? edadMinima = null, int? edadMaxima = null, int? tipoModalidad = null,
            bool? matriculaLinea = false, int? descuento = null, DateTime? fechaInicioDescuento = null, DateTime? fechaFinDescuento = null, bool? esImportado = false, bool? aplicaPrenscripcion = null, int? consecutivoImagen = null, int? cupoMaximoPago = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaaca_codigo", codigoPlan },
                { "plaaca_nombre", nombre },
                { "plaaca_asi_codigo", asignatura },
                { "plaaca_asivig_consecutivo", vigencia },
                { "plaaca_per_consecutivo", periodo },
                { "tipmod_consecutivoP", tipoModalidad },
                { "plaaca_sedjorprogra_consecutivo", sedeJornadaPrograma },
                { "plaaca_cupo_maximo", cupoMaximo },
                { "plaaca_fecha_inicio", fechaInicio },
                { "plaaca_fecha_fin", fechaFin },
                { "plaaca_per_codigo_docente", docente },
                { "plaaca_propen_consecutivo", pensumPrograma },
                { "plaaca_mod_codigo", modulo },
                { "plaaca_porcentaje_comision_docente", porcentajeComision },
                { "plaaca_centro_costos", centroCosto },
                { "plaaca_modalidad", modalidad },
                { "plaaca_codigo_virtual", codigoVirtual},
                { "plaaca_centro_operaciones", centroCostos},
                { "aud_usuario_sesion", per_codigoP },
                { "aud_ip",ClientIpAddress },
                { "aud_cat_codigo", CategoriaAuditoria },
                { "aud_usuario_q10", codigo_ejecutivo_institucion },
                { "aud_interno", false },
                { "plaaca_inf_codigo", infCodigo },
                { "plaaca_opt_codigo", optativa },
                //Personalización de Cajicá
                { "plaaca_edad_minima", edadMinima },
                { "plaaca_edad_maxima", edadMaxima },
                //Comfamiliar
                { "plaaca_aplica_matricula_linea", matriculaLinea },
                { "plaaca_des_consecutivo", descuento },
                { "plaaca_fecha_inicio_descuento", fechaInicioDescuento },
                { "plaaca_fecha_fin_descuento", fechaFinDescuento },
                //Campo que permite determinar si un curso fue creado con plantilla de importación masiva de cursos
                { "plaaca_importacion", esImportado },
                //Campos personalización Colombo
                { "plaaca_aplica_preinscripcion", aplicaPrenscripcion },
                { "plaaca_arc_consecutivo", consecutivoImagen},
                { "plaaca_cupo_maximo_pago", cupoMaximoPago }
            };

            var result = institucion.BaseDatos.ExecuteNonQuery("pa_aca_planes_academicos_trabajo_superior_ingresar", parametros);

            AsyncHelpers.RunSync(() => new ServicioSuggestions(institucion, usuario).ActualizarAccesoDocentesAdministrativos(docente, "D"));

            return result;
        }

        public void ModificarPlanAcademicoSuperior(int consecutivoPlan, string codigoPlan, string nombre, string docente,
            int? sedjorprogra_consecutivo, int? pensumPrograma, string asignatura, int? vigencia, int periodo,
            DateTime? fechaInicio, DateTime? fechaFin, int? cupoMaximo, string modulo, decimal? porcentajeComision = null,
            string codigoVirtual = null, string centroCostos = null, string centroCosto = null, char? modalidad = null,
            string docenteAnterior = null, string infCodigo = null, string optativa = null, int? edadMinima = null, int? edadMaxima = null, int? tipoModalidad = null,
            bool? matriculaLinea = false, int? descuento = null, DateTime? fechaInicioDescuento = null, DateTime? fechaFinDescuento = null, bool? aplicaPrenscripcion = null,
            int? consecutivoImagen = null, int? cupoMaximoPago = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaaca_consecutivoP", consecutivoPlan },
                { "plaaca_codigo", codigoPlan },
                { "plaaca_nombre", nombre },
                { "plaaca_sedjorprogra_consecutivo", sedjorprogra_consecutivo },
                { "plaaca_asi_codigo", asignatura },
                { "plaaca_asivig_consecutivo", vigencia },
                { "plaaca_per_consecutivo", periodo },
                { "plaaca_cupo_maximo", cupoMaximo },
                { "plaaca_fecha_inicio", fechaInicio },
                { "plaaca_fecha_fin", fechaFin },
                { "plaaca_per_codigo_docente", docente },
                { "plaaca_propen_consecutivo", pensumPrograma },
                { "plaaca_mod_codigo", modulo },
                { "plaaca_porcentaje_comision_docente", porcentajeComision},
                { "plaaca_codigo_virtual", codigoVirtual},
                { "plaaca_centro_operaciones", centroCostos},
                { "plaaca_centro_costos", centroCosto },
                { "plaaca_modalidad", modalidad },
                { "tipmod_consecutivoP", tipoModalidad },
                { "aud_usuario_sesion", per_codigoP },
                { "aud_ip",ClientIpAddress},
                { "aud_cat_codigo", CategoriaAuditoria },
                { "aud_usuario_q10", codigo_ejecutivo_institucion },
                { "aud_interno", false },
                { "plaaca_inf_codigo", infCodigo },
                { "plaaca_opt_codigo", optativa },
                //Personalización de Cajicá
                { "plaaca_edad_minima", edadMinima },
                { "plaaca_edad_maxima", edadMaxima },
                //Comfamiliar
                { "plaaca_aplica_matricula_linea", matriculaLinea },
                { "plaaca_des_consecutivo", descuento },
                { "plaaca_fecha_inicio_descuento", fechaInicioDescuento },
                { "plaaca_fecha_fin_descuento", fechaFinDescuento },
                //Colombo
                { "plaaca_cupo_maximo_pago", cupoMaximoPago },
                { "plaaca_aplica_preinscripcion", aplicaPrenscripcion },
                { "plaaca_arc_consecutivo", consecutivoImagen}
            };

            if (docenteAnterior != docente)
            {
                var estudiantes = AsyncHelpers.RunSync(() => new ServicioSuggestions(institucion, usuario).ObtenerAccesoPersonas(docenteAnterior, "D"));

                institucion.BaseDatos.ExecuteNonQuery("pa_aca_planes_academicos_trabajo_superior_modificar", parametros);

                AsyncHelpers.RunSync(() => new ServicioSuggestions(institucion, usuario).ActualizarAccesoDocentesAdministrativos(docente, "D", null, estudiantes));
            }
            else
            {
                institucion.BaseDatos.ExecuteNonQuery("pa_aca_planes_academicos_trabajo_superior_modificar", parametros);
            }
        }

        public void LimpiarCursosArchivo(int? codigoCurso, int? archivoAdjunto = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP  },
                { "plaaca_consecutivoP", codigoCurso },
                { "arc_consecutivoP", archivoAdjunto }
            };

            institucion.BaseDatos.ExecuteNonQuery("paz_colombo_aca_planes_academicos_archivo_modificar", parametros);
        }

        public void EliminarPlanAcademico(int consecutivoPlan, bool eliminarHorarios = false)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaaca_consecutivoP", consecutivoPlan},
                { "eliminar_horarios", eliminarHorarios },
                { "aud_usuario_sesion", per_codigoP },
                { "aud_ip",ClientIpAddress},
                { "aud_cat_codigo", CategoriaAuditoria },
                { "aud_usuario_q10", codigo_ejecutivo_institucion },
                { "aud_interno", false }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_planes_academicos_eliminar", parametros);
        }

        public void ClonarEstudiantesCurso(int consecutivoOrigen, int consecutivoDestino, string omitir = null, string docenteCurso = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaaca_consecutivoP_origen", consecutivoOrigen },
                { "plaaca_consecutivoP_destino", consecutivoDestino },
                { "aud_usuario_sesion", per_codigoP },
                { "aud_ip", ClientIpAddress },
                { "matpla_para_omitir", omitir}
            };

            var estudiantes = AsyncHelpers.RunSync(() => new ServicioSuggestions(institucion, usuario).ObtenerAccesoPersonas(docenteCurso, "D"));

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_planes_academicos_clonar_estudiantes", parametros);

            AsyncHelpers.RunSync(() => new ServicioSuggestions(institucion, usuario).ActualizarAccesoDocentesAdministrativos(docenteCurso, "D", null, estudiantes));

        }

        public int ClonarHorarioCurso(int consecutivoOrigen, int consecutivoDestino)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaaca_consecutivoP_origen", consecutivoOrigen },
                { "plaaca_consecutivoP_destino", consecutivoDestino },
                { "aud_usuario_sesion", per_codigoP },
                { "aud_ip", ClientIpAddress}
            };

            //Retorna 1 si existe cruce de horarios.
            return institucion.BaseDatos.ExecuteNonQuery("pa_aca_planes_academicos_clonar_horario", parametros);
        }

        public IList<ClonarGrupoTrabajoViewModel> ObtenerExcepcionesClonacion(int plaaca_consecutivoP_origen, string asignatura, string modulo, int? pensum = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaaca_consecutivoP_origen", plaaca_consecutivoP_origen },
                { "asi_codigoP", asignatura },
                { "mod_codigoP", modulo},
                { "propen_consecutivoP", pensum}
            };

            return institucion.BaseDatos.ExecuteQuery<ClonarGrupoTrabajoViewModel>("pa_aca_planes_academicos_clonar_estudiantes_validar", parametros);
        }

        public IList<GrupoTrabajoViewModel> ObtenerHistoricoPlanesAcademicosSuperior(int matriculaPrograma, int periodo, string asignatura, string modulo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "matpro_consecutivoP", matriculaPrograma },
                { "asi_codigoP", asignatura },
                { "mod_codigoP", modulo },
                { "per_consecutivoP", periodo }
            };

            return institucion.BaseDatos.ExecuteQuery<GrupoTrabajoViewModel>("pa_aca_planes_academicos_trabajo_superior_historicos_retornar", parametros);
        }

        public int TrasladarHorario(int consecutivoPlan, int? periodoOrigen, int? periodoDestino)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaaca_consecutivoP", consecutivoPlan },
                { "per_consecutivoP_origen", periodoOrigen },
                { "per_consecutivoP_destino", periodoDestino },
                { "aud_usuario_sesion", per_codigoP },
                { "aud_ip", ClientIpAddress}
            };

            //Retorna 1 si existe cruce de horarios.
            return institucion.BaseDatos.ExecuteNonQuery("pa_aca_planes_academicos_trasladar_horario", parametros);
        }

        public bool ValidarPlanAcademicoModificable(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaaca_consecutivoP", consecutivo }
            };

            return Convert.ToBoolean(institucion.BaseDatos.ExecuteNonQuery("pa_aca_planes_academicos_trabajo_superior_validar_para_modificar", parametros));
        }

        public IList<CursoEncuestaViewModel> ObtenerPlanesAcademicosEncuestasParaAsignar(int encuesta, int? periodo, string asignatura, string modulo, char filtro = 'A')
        {
            var parametros = new Dictionary<string, object>(){
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "enc_consecutivoP",encuesta },
                { "per_consecutivoP", periodo} ,
                { "asi_codigoP", asignatura} ,
                { "mod_codigoP", modulo },
                { "filtar_por", filtro },
            };
            return institucion.BaseDatos.ExecuteQuery<CursoEncuestaViewModel>("pa_enc_encuestas_cursos_para_asignar_retornar", parametros).ToList();
        }

        public string ValidarCruceDocente(int codigo, string docente)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaaca_consecutivoP", codigo },
                { "plaaca_per_codigo_docente", docente },
            };

            var result = institucion.BaseDatos.ExecuteQuery<dynamic>("pa_aca_planes_academicos_modificar_validar", parametros).FirstOrDefault();

            if (result != null && (result.nombre_asignatura != null || result.nombre_modulo != null))
            {
                var asignaturas = result.nombre_asignatura;
                var modulos = result.nombre_modulo;

                return string.Join(",", asignaturas, modulos);
            }

            return string.Empty;
        }

        public bool ValidarCruceClonacion(int curso, DateTime fechaInicio, DateTime fechaFin)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaaca_consecutivoP", curso },
                { "fecha_inicio", fechaInicio },
                { "fecha_fin", fechaFin }
            };

            return Convert.ToBoolean(institucion.BaseDatos.ExecuteNonQuery("pa_aca_planes_academicos_cruce_horario_validar", parametros));
        }

        public void ModificarCorreoEnviadoPlanAcademicoSuperior(int consecutivoPlan, bool correoEnviado, DateTime? fechaEnvio)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaaca_consecutivoP", consecutivoPlan },
                { "plaaca_correo_enviado", correoEnviado },
                { "plaaca_fecha_envio_correo", fechaEnvio }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_planes_academicos_trabajo_superior_envio_correos_modificar", parametros);
        }

        public List<HorarioInasistenciaViewModel> ObtenerPlanAcademicoHorariosInasistencia(int planAcademico)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaaca_consecutivoP", planAcademico }
            };

            return institucion.BaseDatos.ExecuteQuery<HorarioInasistenciaViewModel>("pa_aca_plan_academico_horarios_inasistencia_retornar", parametros).ToList(); ;
        }

        public IList<GrupoTrabajoViewModel> ObtenerOpcionesPlanesAcademicosSuperior(string consecutivos)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaaca_consecutivos", consecutivos }
            };

            return institucion.BaseDatos.ExecuteQuery<GrupoTrabajoViewModel>("pa_aca_planes_academicos_retornar", parametros);
        }

        public void ModificarEstadoArchivacionCursos(string consecutivosPlan, bool archivando)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaaca_consecutivoP", consecutivosPlan },
                { "plaaca_archivado_estado", archivando }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_planes_academicos_estado_archivacion_modificar", parametros);
        }

        public bool ValidarCupoMaximoCurso(int consecutivoCurso)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaaca_consecutivoP", consecutivoCurso },
            };

            // Se retorna en true en caso de que se supere el cupo máximo, si hay cupo disponible se retorna false.
            return Convert.ToBoolean(institucion.BaseDatos.ExecuteNonQuery("pa_aca_planes_academicos_cupo_maximo_retornar", parametros));
        }

        #region APIs

        public async Task<IPagedList<GrupoTrabajoViewModel>> ObtenerPlanesAcademicosColomboAPIAsync(DateTime? fechaInicio = null, DateTime? fechaFin = null, int? consecutivoPeriodo = null, string nombrePeriodo = null, int? consecutivoSedeJornada = null, string codigoPrograma = null, string nombrePrograma = null, string estado = null, int pagina = 1, int tamanioPagina = Constantes.PAGINACION_TAMANO_PAGINA)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaaca_fecha_inicio", fechaInicio },
                { "plaaca_fecha_fin", fechaFin },
                { "plaaca_per_consecutivo", consecutivoPeriodo },
                { "per_nombre", nombrePeriodo },
                { "sedjor_consecutivoP", consecutivoSedeJornada },
                { "pro_codigoP", codigoPrograma },
                { "pro_nombre", nombrePrograma },
                { "plaaca_estado", estado }
            };

            var result = await institucion.BaseDatos.ExecuteQueryAsync<GrupoTrabajoViewModel>("paz_colombo_apis_aca_cursos_retornar", parametros, pagina, tamanioPagina);
            return new StaticPagedList<GrupoTrabajoViewModel>(result.Item1, pagina, tamanioPagina, result.Item2);
        }

        public async Task<IPagedList<GrupoTrabajoViewModel>> ObtenerPlanesAcademicosEvaluacionAPIAsync(int? consecutivoPeriodo = null, string codigoPrograma = null, string codigoAsignaturaModulo = null, int? consecutivoSedeJornada = null, int? Consecutivo_curso = null, char? tipoEvaluacion = null, int pagina = 1, int tamanioPagina = Constantes.PAGINACION_TAMANO_PAGINA)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "per_consecutivoP", consecutivoPeriodo },
                { "pro_codigoP", codigoPrograma },
                { "dynamic_codigoP", codigoAsignaturaModulo },
                { "sedjor_consecutivoP", consecutivoSedeJornada },
                { "plaaca_consecutivoP", Consecutivo_curso },
                { "tipo_evaluacion", tipoEvaluacion }
            };

            var result = await institucion.BaseDatos.ExecuteQueryAsync<GrupoTrabajoViewModel>("pa_apis_aca_planes_academicos_retornar", parametros, pagina, tamanioPagina);
            return new StaticPagedList<GrupoTrabajoViewModel>(result.Item1, pagina, tamanioPagina, result.Item2);
        }

        #endregion APIs

        public IList<GrupoViewModel> ObtenerOpcionesGruposTrabajo(int? periodo, string nivel, int? sedeJornada, string programa = null, bool ? estado = null, bool? activos = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_periodos", periodo },
                { "filtros_nivel", nivel }, 
                { "filtros_sedejornada", sedeJornada }, 
                { "filtros_estado", estado },
                { "filtros_programas", programa }, 
                { "filtro_activos", activos },
            };

            return institucion.BaseDatos.ExecuteQuery<GrupoViewModel>("pa_aca_grupos_trabajos_opciones_retornar", parametros);
        }

        #endregion Grupos Trabajo - Planes académicos

        #region Prerrequisitos

        public IList<AsignaturaViewModel> ObtenerAsignaturasPrerrequisitos(string codigoAsignatura, int consecutivoPensumPrograma, char tipo = '1', string codigoOptativa = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "asi_codigoP", codigoAsignatura },
                { "propen_consecutivoP", consecutivoPensumPrograma },
                { "prerre_tipo", tipo},
                { "opt_codigoP", codigoOptativa}
            };

            return institucion.BaseDatos.ExecuteQuery<AsignaturaViewModel>("pa_aca_prerrequisitos_asignatura_retornar", parametros);
        }

        public IList<AsignaturaViewModel> ObtenerAsignaturasPrerrequisitosModular(string codigoAsignatura, int consecutivoPensumPrograma, char tipo = '1')
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "asi_codigoP", codigoAsignatura },
                { "propen_consecutivoP", consecutivoPensumPrograma },
                { "prerre_tipo", tipo}
            };

            return institucion.BaseDatos.ExecuteQuery<AsignaturaViewModel>("pa_aca_prerrequisitos_asignatura_modular_retornar", parametros);
        }

        public IList<PrerrequisitoViewModel> ObtenerPrerrequisitos(string asignatura, int pensumPrograma, char tipo = '1', string codigoOptativa = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "asi_codigoP", asignatura },
                { "propen_consecutivoP", pensumPrograma },
                { "prerre_tipo", tipo},
                { "opt_codigoP", codigoOptativa }
            };

            return institucion.BaseDatos.ExecuteQuery<PrerrequisitoViewModel>("pa_aca_prerrequisitos_retornar", parametros);
        }

        public void IngresarPrerrequisito(string codigoAsignatura, string codigoPrerrequisito, int PensumPrograma, char tipo = '1', string codigoOptativa = null, string codigoPrerreOptativa = null, char? tipoProceso = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "prerre_asi_codigo", codigoAsignatura },
                { "prerre_asi_prerrequisito", codigoPrerrequisito },
                { "prerre_propen_consecutivo", PensumPrograma },
                { "prerre_tipo", tipo},
                { "prerre_opt_codigo", codigoOptativa },
                { "prerre_opt_prerrequisito", codigoPrerreOptativa },
                //0-null: Ingreso manual, 1: Ingreso desde optativas
                { "prerre_tipo_proceso", tipoProceso }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_prerrequisitos_ingresar", parametros);
        }

        public void EliminarPrerrequisito(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "prerre_consecutivoP", consecutivo }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_prerrequisitos_eliminar", parametros);
        }

        #endregion

        #region Correquisitos

        public IList<AsignaturaViewModel> ObtenerAsignaturasCorrequisitos(string codigoAsignatura, int consecutivoPensumPrograma, string codigoOptativa = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "asi_codigoP", codigoAsignatura },
                { "propen_consecutivoP", consecutivoPensumPrograma },
                { "opt_codigoP", codigoOptativa }
            };

            return institucion.BaseDatos.ExecuteQuery<AsignaturaViewModel>("pa_aca_correquisitos_asignatura_retornar", parametros);
        }

        public IList<AsignaturaViewModel> ObtenerAsignaturasCorrequisitosModular(string codigoAsignatura, int consecutivoPensumPrograma)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "asi_codigoP", codigoAsignatura },
                { "propen_consecutivoP", consecutivoPensumPrograma },
            };

            return institucion.BaseDatos.ExecuteQuery<AsignaturaViewModel>("pa_aca_correquisitos_asignatura_modular_retornar", parametros);
        }

        public IList<CorrequisitoViewModel> ObtenerCorrequisitos(string asignatura, int pensumPrograma, string codigoOptativa = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "asi_codigoP", asignatura },
                { "propen_consecutivoP", pensumPrograma },
                { "opt_codigo", codigoOptativa }
            };

            return institucion.BaseDatos.ExecuteQuery<CorrequisitoViewModel>("pa_aca_correquisitos_retornar", parametros);
        }

        public void IngresarCorrequisito(string codigoAsignatura, string codigoCorrequisito, int PensumPrograma, string codigoOptativa = null, string codigoCorreqOptativa = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "corre_asi_codigo", codigoAsignatura },
                { "corre_asi_correquisito", codigoCorrequisito },
                { "corre_propen_consecutivo", PensumPrograma },
                { "corre_opt_codigo", codigoOptativa },
                { "corre_opt_correquisito", codigoCorreqOptativa }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_correquisitos_ingresar", parametros);
        }

        public void EliminarCorrequisito(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "corre_consecutivoP", consecutivo }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_correquisitos_eliminar", parametros);
        }

        public int ValidarCorrequisitosAsignaturas(int consecutivo)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "premat_consecutivoP", consecutivo }
            };

            var correquisito = institucion.BaseDatos.ExecuteQuery<dynamic>("pa_aca_prematriculas_correquisitos_eliminar_validar", parametros).FirstOrDefault();

            if (correquisito == null) return 0;

            return correquisito.correquisito;
        }

        /// <summary>
        /// Dado un correquisito, devuelve el código de la asignatura principal, donde exista la relación
        /// </summary>
        public string ObtenerCorrequisitoAsignaturaPrincipal(string asignaturaCorrequisito, int pensum)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "asi_codigoP", asignaturaCorrequisito },
                { "propen_consecutivoP", pensum }
            };

            var result = institucion.BaseDatos.ExecuteQuery<dynamic>("pa_aca_correquisitos_asignatura_principal_retornar", parametros).FirstOrDefault();

            return result?.corre_asi_codigo ?? string.Empty;
        }

        #endregion Correquisitos

        #region Sedes - Jornadas - Programas

        public IList<ProgramaSedeJornadaViewModel> ObtenerSedesJornadasPrograma(string Programa)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "pro_codigoP", Programa }
            };

            return institucion.BaseDatos.ExecuteQuery<ProgramaSedeJornadaViewModel>("pa_par_programas_sedes_jornadas_retornar", parametros);
        }

        public IList<ProgramaViewModel> ObtenerProgramasAsignar(int consecutivoSedeJornada, bool estado)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP",institucion.aplent_codigoP },
                { "sedjorprogra_sedjor_consecutivo", consecutivoSedeJornada },
                { "pro_estado", estado }
            };

            return institucion.BaseDatos.ExecuteQuery<ProgramaViewModel>("pa_aca_programas_para_asignar_retornar", parametros);
        }

        public IList<ProgramaSedeJornadaViewModel> ObtenerProgramasSedesJornadas(int? consecutivoSedeJornada, bool? estado = null, string sede = null, bool? estadoJornada = null, bool? inactivos = null, char? categoriaAfiliado = null, bool? estadoSedeJornada = null, string codigoTipoPrograma = null)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP",institucion.aplent_codigoP },
                { "sedjor_consecutivoP", consecutivoSedeJornada },
                { "filtros_estado", estado },// true solo los de pensum activo, null todos los programas
                { "sed_codigoP", sede },
                { "jor_estado", estadoJornada },
                { "per_codigoP", per_codigoP },
                { "filtros_inactivo", inactivos },
                { "rol_codigoP", rol_codigoP },
                { "pro_tippro_codigo", codigoTipoPrograma},
                //Campo personalizado para Cafam, que solo retorne los programas que aplican para la categoría del afiliado
                { "per_categoria", categoriaAfiliado },
                { "sedjor_estado", estadoSedeJornada}
            };

            return institucion.BaseDatos.ExecuteQuery<ProgramaSedeJornadaViewModel>("pa_par_sedes_jornadas_programas_retornar", parametros);
        }

        public IPagedList<ProgramaSedeJornadaViewModel> ObtenerProgramasSedeJornada(int? consecutivoSedeJornada, bool? inactivos, int paginaIndex = 1, int tamanioPagina = Constantes.PAGINACION_TAMANO_PAGINA)
        {
            int totalCount;
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP",institucion.aplent_codigoP },
                { "sedjor_consecutivoP", consecutivoSedeJornada },
                { "per_codigoP", per_codigoP },
                { "filtros_inactivo", inactivos },
                { "rol_codigoP", rol_codigoP }
            };

            var resultado = institucion.BaseDatos.ExecuteQuery<ProgramaSedeJornadaViewModel>("pa_par_sedes_jornadas_programas_retornar", parametros, paginaIndex, tamanioPagina, out totalCount);
            return new StaticPagedList<ProgramaSedeJornadaViewModel>(resultado, paginaIndex, tamanioPagina, totalCount);
        }

        public IList<ProgramaSedeJornadaViewModel> ObtenerProgramasSedesJornadasMasivas(string consecutivoSedesJornadas, bool? estado = null, bool? estadoJornada = null, char? tipo = null, bool? sedjorprogra_estado = true)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP",institucion.aplent_codigoP },
                { "sedes_jornadas", consecutivoSedesJornadas },
                { "filtros_estado", estado },// true solo los de pensum activo, null todos los programas
                { "jor_estado", estadoJornada },
                { "pro_tipo_evaluacion", tipo },
                { "sedjorprogra_estado", sedjorprogra_estado}
            };

            return institucion.BaseDatos.ExecuteQuery<ProgramaSedeJornadaViewModel>("pa_par_sedes_jornadas_programas_masivo_retornar", parametros);
        }

        public IList<ProgramaSedeJornadaViewModel> ObtenerProgramasSedesJornadasColombo(string consecutivoSedesJornadas)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "sedjor_consecutivoP", consecutivoSedesJornadas },
            };

            return institucion.BaseDatos.ExecuteQuery<ProgramaSedeJornadaViewModel>("paz_colombo_aca_programas_masivo_retornar", parametros);
        }

        public async Task<IList<ProgramaSedeJornadaViewModel>> ObtenerProgramasSedesJornadasAsync(int? consecutivoSedeJornada, bool? estado = null, string sede = null, bool? estadoJornada = null, bool? inactivos = null, char? categoriaAfiliado = null)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP",institucion.aplent_codigoP },
                { "sedjor_consecutivoP", consecutivoSedeJornada },
                { "filtros_estado", estado },// true solo los de pensum activo, null todos los programas
                { "sed_codigoP", sede },
                { "jor_estado", estadoJornada },
                { "per_codigoP", per_codigoP },
                { "filtros_inactivo", inactivos },
                { "rol_codigoP", rol_codigoP },
                //Campo personalizado para Cafam, que solo retorne los programas que aplican para la categoría del afiliado
                { "per_categoria", categoriaAfiliado }
            };

            return await institucion.BaseDatos.ExecuteQueryAsync<ProgramaSedeJornadaViewModel>("pa_par_sedes_jornadas_programas_retornar", parametros);
        }

        public IList<ProgramaSedeJornadaViewModel> ObtenerProgramasSedesJornadasDocente(int? consecutivoSedeJornada, bool? estado = null, string sede = null, bool? estadoJornada = null)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP",institucion.aplent_codigoP },
                { "sedjor_consecutivoP", consecutivoSedeJornada },
                { "filtros_estado", estado },// true solo los de pensum activo, null todos los programas
                { "sed_codigoP", sede },
                { "jor_estado", estadoJornada },
                { "per_codigoP", per_codigoP },
                { "rol_codigoP", rol_codigoP }
            };

            return institucion.BaseDatos.ExecuteQuery<ProgramaSedeJornadaViewModel>("paz_amazonia_par_sedes_jornadas_programas_retornar", parametros);
        }

        public ProgramaSedeJornadaViewModel ObtenerDetalleProgramaSedeJornada(int consecutivoSedeJornadaPrograma)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP",institucion.aplent_codigoP },
                { "sedjorprogra_consecutivoP", consecutivoSedeJornadaPrograma }
            };

            return institucion.BaseDatos.ExecuteQuery<ProgramaSedeJornadaViewModel>("pa_par_sedes_jornadas_programas_detalle_retornar", parametros).FirstOrDefault();
        }

        public void IngresarProgramaSedeJornada(int codigoSedeJornada, string codigoPrograma, bool sedeJornada)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP",institucion.aplent_codigoP },
                { "sedjorprogra_sedjor_consecutivo", codigoSedeJornada },
                { "sedjorprogra_pro_codigo", codigoPrograma },
                { "sedjorprogra_estado", sedeJornada }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_par_sedes_jornadas_programas_ingresar", parametros);
        }

        public IList<ProgramaSedeJornadaViewModel> ObtenerProgramasSedesJornadasParaInscripcion(int? consecutivoSedeJornada, string persona, bool? aplicaPreinscripcion = null)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP",institucion.aplent_codigoP },
                { "sedjor_consecutivoP", consecutivoSedeJornada },
                { "per_codigoP", persona },
                { "pro_aplica_preinscripcion", aplicaPreinscripcion }
            };

            return institucion.BaseDatos.ExecuteQuery<ProgramaSedeJornadaViewModel>("pa_aca_programas_para_asignar_inscripcion_retornar", parametros);
        }

        public void InactivarProgramasSedesJornadas(int? consecutivoSedeJornada, bool? activo)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP",institucion.aplent_codigoP },
                { "sedjorprogra_consecutivoP", consecutivoSedeJornada },
                { "sedjorprogra_estado", activo }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_par_sedes_jornadas_programas_inactivar", parametros);
        }

        public IList<ProgramaSedeJornadaViewModel> ObtenerOpcionesProgramasSedesJornadas(int? consecutivoSedeJornada, bool? estado = null, string sede = null, bool? estadoJornada = null, bool? inactivos = null, char? categoriaAfiliado = null, bool? estadoSedeJornada = null, string codigoTipoPrograma = null)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP",institucion.aplent_codigoP },
                { "sedjor_consecutivoP", consecutivoSedeJornada },
                { "filtros_estado", estado },// true solo los de pensum activo, null todos los programas
                { "sed_codigoP", sede },
                { "jor_estado", estadoJornada },
                { "per_codigoP", per_codigoP },
                { "rol_codigoP", rol_codigoP },
                { "sedjor_estado", estadoSedeJornada},
                { "filtros_inactivo", inactivos },
                { "per_categoria", categoriaAfiliado },//Campo personalizado para Cafam, que solo retorne los programas que aplican para la categoría del afiliado
                { "pro_tippro_codigo", codigoTipoPrograma}
            };

            return institucion.BaseDatos.ExecuteQuery<ProgramaSedeJornadaViewModel>("pa_par_sedes_jornadas_programas_opciones_retornar", parametros);
        }

        #endregion

        #region Sedes - Jornadas - Grados

        public bool ValidarRelacionSedeGrado(string codigoSede, int consecutivoGrado)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "sed_codigoP", codigoSede },
                { "gra_consecutivoP", consecutivoGrado }
            };

            return Convert.ToBoolean(institucion.BaseDatos.ExecuteNonQuery("pa_par_sedes_jornadas_programas_grados_validar", parametros));
        }

        public IList<GradoSedeJornadaViewModel> ObtenerSedesJornadasGrado(int grado)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gra_consecutivoP", grado }
            };

            return institucion.BaseDatos.ExecuteQuery<GradoSedeJornadaViewModel>("pa_par_grados_sedes_jornadas_retornar", parametros);
        }

        public IList<GradoViewModel> ObtenerGradosAsignar(int consecutivoSedeJornada)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP",institucion.aplent_codigoP },
                { "sedjorprogra_sedjor_consecutivo", consecutivoSedeJornada}
            };

            return institucion.BaseDatos.ExecuteQuery<GradoViewModel>("pa_aca_grados_para_asignar_retornar", parametros);
        }

        public IList<GradoSedeJornadaViewModel> ObtenerGradosSedesJornadas(int? consecutivoSedeJornada, string codigoSede = null, int? jornada = null, bool? estadoJornada = null, bool? estadoGrado = null, char? filtro = null, bool omitirPersona = false, bool? inactivos = true)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP",institucion.aplent_codigoP },
                { "sedjor_consecutivoP", consecutivoSedeJornada },
                { "sed_codigoP", codigoSede },
                { "rol_codigoP", !omitirPersona ? rol_codigoP : null},
                { "per_codigoP", !omitirPersona ? per_codigoP : null},
                { "jor_estado", estadoJornada },
                { "gra_estado", estadoGrado },
                { "filtro_tipo", filtro },
                { "filtros_inactivo", inactivos }
            };

            return institucion.BaseDatos.ExecuteQuery<GradoSedeJornadaViewModel>("pa_par_sedes_jornadas_grados_retornar", parametros);
        }

        public IPagedList<GradoSedeJornadaViewModel> ObtenerGradosSedeJornada(int? consecutivoSedeJornada, bool? inactivos = true, int paginaIndex = 1)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP",institucion.aplent_codigoP },
                { "sedjor_consecutivoP", consecutivoSedeJornada },
                { "rol_codigoP", rol_codigoP },
                { "per_codigoP", per_codigoP },
                { "filtros_inactivo", inactivos }
            };

            var resultado = institucion.BaseDatos.ExecuteQuery<GradoSedeJornadaViewModel>("pa_par_sedes_jornadas_grados_retornar", parametros, paginaIndex, Constantes.PAGINACION_TAMANO_PAGINA, out int totalCount);
            return new StaticPagedList<GradoSedeJornadaViewModel>(resultado, paginaIndex, Constantes.PAGINACION_TAMANO_PAGINA, totalCount);
        }

        public IList<GradoSedeJornadaViewModel> ObtenerGradosSedesJornadasMasivas(string consecutivoSedesJornadas, bool? estado = null, bool? estadoJornada = null)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP",institucion.aplent_codigoP },
                { "sedes_jornadas", consecutivoSedesJornadas },
                { "filtros_estado", estado },// true solo los de pensum activo, null todos los programas
                { "jor_estado", estadoJornada }
            };

            return institucion.BaseDatos.ExecuteQuery<GradoSedeJornadaViewModel>("pa_par_sedes_jornadas_grados_masivo_retornar", parametros);
        }

        public GradoSedeJornadaViewModel ObtenerDetalleGradoSedeJornada(int consecutivo)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP",institucion.aplent_codigoP },
                { "sedjorprogra_consecutivoP", consecutivo}
            };

            return institucion.BaseDatos.ExecuteQuery<GradoSedeJornadaViewModel>("pa_par_sedes_jornadas_grados_detalle_retornar", parametros).FirstOrDefault();
        }

        public void IngresarGradoSedeJornada(int codigoSedeJornada, int codigoGrado, string centroCostos = null, string codigoProyecto = null, bool estadoGradoSedJor = false)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP",institucion.aplent_codigoP },
                { "sedjorprogra_sedjor_consecutivo", codigoSedeJornada },
                { "sedjorprogra_gra_consecutivo", codigoGrado },
                { "sedjorprogra_centro_costo", centroCostos },//Campo personalizado Comfanorte
                { "sedjorprogra_codigo_proyecto", codigoProyecto },//Campo personalizado Comfanorte
                { "sedjorprogra_estado", estadoGradoSedJor }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_par_sedes_jornadas_grados_ingresar", parametros);
        }

        public void ModificarGradoSedeJornada(int consecutivo, string centroCostos = null, string codigoProyecto = null, bool estadoGradoSedJor = false)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP",institucion.aplent_codigoP },
                { "sedjorprogra_consecutivoP", consecutivo },
                { "sedjorprogra_centro_costo", centroCostos },//Campo personalizado Comfanorte
                { "sedjorprogra_codigo_proyecto", codigoProyecto },//Campo personalizado Comfanorte
                { "sedjorprogra_estado", estadoGradoSedJor }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_par_sedes_jornadas_grados_modificar", parametros);
        }

        public void EliminarSedeJornadaProgramaGrado(int consecutivo)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP",institucion.aplent_codigoP },
                { "sedjorprogra_consecutivoP", consecutivo}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_par_sedes_jornadas_programas_grados_eliminar", parametros);
        }

        public async Task<int?> ObtenerSedeJornadaGradoAsync(int consecutivoGrado, int consecutivoSedeJornada, bool? estado = null)
        {
            var parametros = new Dictionary<string, object>()
            {
               { "aplent_codigoP",institucion.aplent_codigoP },
               { "gra_consecutivoP", consecutivoGrado },
               { "sedjor_consecutivoP", consecutivoSedeJornada },
               { "filtro_estado", estado }
            };

            return (await institucion.BaseDatos.ExecuteQueryAsync<int?>("pa_apis_par_grado_sede_jornada_retornar", parametros)).FirstOrDefault();
        }

        public IList<GradoSedeJornadaViewModel> ObtenerGradosSedesMasivo(string codigoSedes)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP",institucion.aplent_codigoP },
                { "sed_codigos", codigoSedes },
            };

            return institucion.BaseDatos.ExecuteQuery<GradoSedeJornadaViewModel>("pa_aca_grados_por_sedes_retornar", parametros);
        }

        #endregion Sedes - Jornadas - Grados

        #region Areas

        public IList<AreaViewModel> ObtenerAreas(string texto)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_texto", texto }
            };

            return institucion.BaseDatos.ExecuteQuery<AreaViewModel>("pa_aca_areas_retornar", parametros);
        }

        public AreaViewModel ObtenerDetalleArea(string codigo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "are_codigoP", codigo }
            };

            return institucion.BaseDatos.ExecuteQuery<AreaViewModel>("pa_aca_areas_detalle_retornar", parametros).FirstOrDefault();
        }

        public void IngresarArea(string codigoArea, string nombre, bool promocion, int? ordenamiento, string especialidad)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "are_codigoP", codigoArea },
                { "are_nombre", nombre },
                { "are_promocion", promocion },
                { "are_ordenamiento", ordenamiento },
                { "are_esp_codigo", especialidad}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_areas_ingresar", parametros);
        }

        public void ModificarArea(string codigoArea, string nombre, bool promocion, int? ordenamiento, string especialidad)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "are_codigoP", codigoArea },
                { "are_nombre", nombre },
                { "are_promocion", promocion },
                { "are_ordenamiento", ordenamiento },
                { "are_esp_codigo", especialidad}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_areas_modificar", parametros);
        }

        public void EliminarArea(string codigoArea)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "are_codigoP", codigoArea}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_areas_eliminar", parametros);
        }

        public bool ValidarEstudiantesMatriculadosArea(string codigoArea)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "are_codigoP", codigoArea }
            };

            return Convert.ToBoolean(institucion.BaseDatos.ExecuteNonQuery("pa_aca_areas_validar", parametros));
        }

        #region API Colegios - Areas

        public async Task<IPagedList<AreaViewModel>> ObtenerAreasApi(string codigo = null, bool? aplicaPromocion = null, int pagina = 1, int tamanioPagina = Constantes.PAGINACION_TAMANO_PAGINA)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "are_codigo", codigo },
                { "are_promocion", aplicaPromocion }
            };

            var result = await institucion.BaseDatos.ExecuteQueryAsync<AreaViewModel>("pa_apis_aca_areas_retornar", parametros, pagina, tamanioPagina);
            return new StaticPagedList<AreaViewModel>(result.Item1, pagina, tamanioPagina, result.Item2);
        }

        public async Task<AreaViewModel> ObtenerDetalleAreaApi(string codigo, bool? aplicaPromocion = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "are_codigo", codigo },
                { "are_promocion", aplicaPromocion }
            };

            return (await institucion.BaseDatos.ExecuteQueryAsync<AreaViewModel>("pa_apis_aca_areas_retornar", parametros)).FirstOrDefault();
        }

        public async Task<IList<AsignaturaViewModel>> ObtenerAsignaturasAreasApi(string codigosAreas)
        {
            var parameters = new Dictionary<string, object>()
            {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "are_codigos",  codigosAreas }
            };

            return await institucion.BaseDatos.ExecuteQueryAsync<AsignaturaViewModel>("pa_apis_aca_areas_asignaturas_retornar", parameters);
        }

        public int IngresarAreaApi(string codigoArea, string nombre, bool? promocion, int? ordenamiento, string especialidad)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "are_codigoP", codigoArea },
                { "are_nombre", nombre },
                { "are_promocion", promocion },
                { "are_ordenamiento", ordenamiento },
                { "are_esp_codigo", especialidad}
            };

            return institucion.BaseDatos.ExecuteNonQuery("pa_apis_aca_areas_ingresar", parametros);
        }

        # endregion API Colegios - Areas

        #endregion Areas

        #region Especialidades

        public IList<EspecialidadViewModel> ObtenerEspecialidades(bool? estado, string texto)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_estado", estado },
                { "filtros_texto", texto }
            };

            return institucion.BaseDatos.ExecuteQuery<EspecialidadViewModel>("pa_aca_especialidades_retornar", parametros);
        }

        public EspecialidadViewModel ObtenerDetalleEspecialidad(string codigo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "esp_codigoP", codigo }
            };

            return institucion.BaseDatos.ExecuteQuery<EspecialidadViewModel>("pa_aca_especialidades_detalle_retornar", parametros).FirstOrDefault();
        }

        public void IngresarEspecialidad(string codigo, string nombre, bool estado)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "esp_codigoP", codigo },
                { "esp_nombre", nombre },
                { "esp_estado", estado }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_especialidades_ingresar", parametros);
        }

        public void ModificarEspecialidad(string codigo, string nombre, bool estado)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
               { "esp_codigoP", codigo },
                { "esp_nombre", nombre },
                { "esp_estado", estado }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_especialidades_modificar", parametros);
        }

        public void EliminarEspecialidad(string codigo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "esp_codigoP", codigo }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_especialidades_eliminar", parametros);
        }

        #region Apis colegios - Especialidades

        public async Task<IPagedList<EspecialidadViewModel>> ObtenerEspecialidadesApi(bool? estado, int pagina = 1, int tamanioPagina = Constantes.PAGINACION_TAMANO_PAGINA)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_estado", estado }
            };

            var result = await institucion.BaseDatos.ExecuteQueryAsync<EspecialidadViewModel>("pa_api_aca_especialidades_retornar", parametros, pagina, tamanioPagina);
            return new StaticPagedList<EspecialidadViewModel>(result.Item1, pagina, tamanioPagina, result.Item2);
        }

        public async Task<EspecialidadViewModel> ObtenerDetalleEspecialidadApiAsync(string codigo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "esp_codigoP", codigo }
            };

            return (await institucion.BaseDatos.ExecuteQueryAsync<EspecialidadViewModel>("pa_apis_aca_especialidades_detalle_retornar", parametros)).FirstOrDefault();
        }

        #endregion Apis colegios - Especialidades

        #endregion Especialidades

        #region Grupos Colegios/Salud

        public IList<GrupoViewModel> ObtenerGruposGrado(int gradoSedeJornada, int? periodo, char? filtro = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "sedjorprogra_consecutivoP", gradoSedeJornada },
                { "per_consecutivoP", periodo },
                { "per_codigoP", per_codigoP },
                { "rol_codigoP", rol_codigoP },
                { "filtro_tipo", filtro }
            };

            return institucion.BaseDatos.ExecuteQuery<GrupoViewModel>("pa_aca_grupos_sede_jornada_grado_retornar", parametros);
        }

        public IList<GrupoViewModel> ObtenerGrupos(string texto, int? gradoConsecutivo, int? sedeJornada, int? periodo, int? programaPensum, string nivel, bool? estado = null, bool? archivados = false, string programa = null, char? rolPersona = null, string codigoPersona = null, string sede = null, char? filtro = null, bool? activos = null, int? sedeJornadaPrograma = null)
        {
            var tipoEvaluacion = institucion.ObtenerParametro<string>(Constantes.TIPO_EVALUACION);

            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_grados", gradoConsecutivo },
                { "filtros_texto", texto },
                { "filtros_periodos", periodo },
                { "filtros_nivel", nivel },
                { "filtros_sedejornada", sedeJornada },
                { "filtros_programa_pensum", programaPensum },
                { "filtros_tipo", tipoEvaluacion },
                { "filtros_estado", estado },
                { "filtros_archivado",archivados },
                { "filtros_programas", programa },
                { "filtros_sede", sede },
                { "rol_codigoP", rolPersona },
                { "per_codigoP", codigoPersona },
                { "filtro_tipo", filtro },
                { "filtro_activos", activos },
                { "sedjorprogra_consecutivoP", sedeJornadaPrograma }
            };

            return institucion.BaseDatos.ExecuteQuery<GrupoViewModel>("pa_aca_grupos_retornar", parametros);
        }

        public GrupoViewModel ObtenerDetalleGrupo(int? consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gru_consecutivoP", consecutivo }
            };

            return institucion.BaseDatos.ExecuteQuery<GrupoViewModel>("pa_aca_grupos_detalle_retornar", parametros).FirstOrDefault();
        }

        public async Task<GrupoViewModel> ObtenerDetalleGrupoAsync(int? consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gru_consecutivoP", consecutivo }
            };

            return (await institucion.BaseDatos.ExecuteQueryAsync<GrupoViewModel>("pa_aca_grupos_detalle_retornar", parametros)).FirstOrDefault();
        }

        public string ExisteConflictoHorarioGrupo(int planAcademico, int idGrupo, int? planActual)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaaca_consecutivoP", planAcademico },
                { "gru_consecutivoP", idGrupo },
                { "plaaca_actual", planActual },
            };
            var result = institucion.BaseDatos.ExecuteQuery<dynamic>("pa_aca_grupo_validar_horario_retornar", parametros).FirstOrDefault();
            if (result == null) return string.Empty;

            return result.nombre;
        }

        public int IngresarGrupo(string codigo, string nombre, int? sedeJornadaProgramaGrado, int? periodo, string persona, int? pensumPrograma, string nivel, DateTime? fechaInicio, DateTime? gruren_fecha_amortizacion, int? cupoMaximo, bool? estado)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gru_codigo" , codigo },
                { "gru_nombre" , nombre },
                { "gru_sedjorprogra_consecutivo" , sedeJornadaProgramaGrado },
                { "gru_per_consecutivo" , periodo },
                { "gru_propen_consecutivo", pensumPrograma },
                { "gru_per_codigo_coordinador", persona },
                { "gru_niv_codigo" , nivel },
                { "gru_fecha_inicio",fechaInicio },
                { "gru_fecha_amortizacion", gruren_fecha_amortizacion },
                { "gru_cupo_maximo ", cupoMaximo },
                { "gru_activo", estado }
            };

            var id = institucion.BaseDatos.ExecuteQuery<GrupoViewModel>("pa_aca_grupos_ingresar", parametros).FirstOrDefault();

            AsyncHelpers.RunSync(async () => await new ServicioSuggestions(institucion, usuario).ActualizarAccesoDocentesAdministrativos(persona, "D"));

            return id.gru_consecutivoP;
        }

        public void ModificarGrupo(int consecutivo, string codigo, string nombre, int? sedeJornadaProgramaGrado, int? periodo, string persona, int? pensumPrograma, string nivel, DateTime? gruren_fecha_amortizacion, int? cupoMaximo, bool? estado)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gru_consecutivoP", consecutivo },
                { "gru_codigo", codigo },
                { "gru_nombre", nombre },
                { "gru_sedjorprogra_consecutivo", sedeJornadaProgramaGrado },
                { "gru_per_consecutivo", periodo },
                { "gru_propen_consecutivo", pensumPrograma },
                { "gru_per_codigo_coordinador", persona },
                { "gru_niv_codigo", nivel },
                { "gru_fecha_amortizacion", gruren_fecha_amortizacion },
                { "gru_cupo_maximo ", cupoMaximo },
                { "gru_activo", estado },
                { "aud_usuario_sesion", per_codigoP },
                { "aud_ip", ClientIpAddress },
                { "aud_cat_codigo", CategoriaAuditoria },
                { "aud_usuario_q10", codigo_ejecutivo_institucion },
                { "aud_interno", false }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_grupos_modificar", parametros);
        }

        public void EliminarGrupo(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gru_consecutivoP", consecutivo }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_grupos_eliminar", parametros);
        }

        public void ModificarEstadoGrupo(int consecutivo, bool estado, string tipoFiltroAuditoria)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gru_consecutivoP" , consecutivo },
                { "gru_estado" , estado },
                //Campos auditoria
                { "aud_usuario_sesion", per_codigoP },
                { "aud_ip", ClientIpAddress },
                { "aud_cat_codigo", CategoriaAuditoria },
                { "aud_usuario_q10", codigo_ejecutivo_institucion },
                { "aud_interno", false },
                { "aud_tipo", tipoFiltroAuditoria }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_grupos_cerrar", parametros);
        }

        public IList<GrupoTrabajoViewModel> ObtenerPlanesAcademicosParaAsignarGrupo(int? periodo, int? sedeJornada, string asignatura, int? grupo, string modulo, bool? incluir_correo_enviado = null, string variablePersonalizada = null, char? tipoEvaluacion = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_periodo", periodo },
                { "filtros_sedejornada", sedeJornada },
                { "filtros_asignatura", asignatura },
                { "filtro_modulos", modulo },
                { "filtros_grupo", grupo },
                { "tipo_evaluacion", tipoEvaluacion },
                { "plaaca_correo_enviado", incluir_correo_enviado },
                { "pro_variable1", variablePersonalizada }
            };

            return institucion.BaseDatos.ExecuteQuery<GrupoTrabajoViewModel>("pa_aca_planes_academicos_trabajo_superior_disponibles_por_grupo_retornar", parametros);
        }

        public IList<EstudianteGrupoViewModel> ObtenerEstudiantesGrupoCurso(int grupoRenovacion, string asignatura, int? curso, string texto, string modulo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gruren_consecutivoP", grupoRenovacion },
                { "asi_codigoP", asignatura },
                { "mod_codigoP", modulo },
                { "plaaca_consecutivoP", curso },
                { "filtro_texto", texto },
                { "gru_consecutivoP", null }
            };

            return institucion.BaseDatos.ExecuteQuery<EstudianteGrupoViewModel>("pa_aca_grupos_estudiantes_trabajo_superior_estado_asignatura_retornar", parametros);
        }

        public void ModificarCursoPredeterminadoGrupo(int grupoAsignaturaCurso)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gruasicur_consecutivoP" , grupoAsignaturaCurso }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_grupos_asignaturas_cursos_predeterminado_modificar", parametros);
        }

        public bool ValidarCodigoRepetido(int? periodo, string codigo, int? consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "per_consecutivoP", periodo },
                { "gru_codigo", codigo },
                { "gru_consecutivoP", consecutivo }
            };

            return Convert.ToBoolean(institucion.BaseDatos.ExecuteNonQuery("pa_aca_grupos_por_codigo_validar", parametros));
        }

        public IList<GrupoViewModel> ObtenerGruposEstudiante(string estudiante, int? grado, int? periodo)
        {

            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "per_codigoP", estudiante },
                { "gra_consecutivoP", grado },
                { "per_consecutivoP", periodo },
            };

            return institucion.BaseDatos.ExecuteQuery<GrupoViewModel>("pa_aca_grados_estudiante_historicos_retornar", parametros);
        }

        public IList<GrupoViewModel> ObtenerGruposDocente(string docente, int? grado, int? periodo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "per_codigoP", docente },
                { "gra_consecutivoP", grado },
                { "per_consecutivoP", periodo },
            };

            return institucion.BaseDatos.ExecuteQuery<GrupoViewModel>("pa_aca_grupos_historicos_docentes_retornar", parametros); 
        }

        public IList<GrupoViewModel> ObtenerGruposGradosMasivo(string consecutivoGrados, int periodo)
        {

            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "per_consecutivoP", periodo },
                { "gra_consecutivos", consecutivoGrados }
            };

            return institucion.BaseDatos.ExecuteQuery<GrupoViewModel>("pa_aca_grupos_por_anio_lectivo_grados_retornar", parametros);
        }

        #region API

        public async Task<IPagedList<GrupoViewModel>> ObtenerGruposAPIAsync(int pagina, int totalPagina, int? periodo = null, string programa = null, int? sedeJornada = null, string nivel = null, bool? estado = null)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gru_per_consecutivo", periodo },
                { "pro_codigoP", programa },
                { "sedjor_consecutivoP", sedeJornada },
                { "gru_niv_codigo", nivel },
                { "filtro_activo", estado }
            };

            var result = await institucion.BaseDatos.ExecuteQueryAsync<GrupoViewModel>("pa_APIS_aca_grupos_retornar", parametros, pagina, totalPagina);
            return new StaticPagedList<GrupoViewModel>(result.Item1, pagina, totalPagina, result.Item2);
        }
        #region ApisGruposColegios

        public async Task<IPagedList<EscalaNacionalViewModel>> ObtenerEscalaNacionalAPIAsync(int? periodoConsecutivo, int pagina = 1, int tamanioPagina = Constantes.PAGINACION_TAMANO_PAGINA)
        {
            var parametros = new Dictionary<string, object>(){
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "escnac_per_consecutivo", periodoConsecutivo }
            };

            var result = await institucion.BaseDatos.ExecuteQueryAsync<EscalaNacionalViewModel>("pa_apis_aca_escalas_nacionales_retornar", parametros, pagina, tamanioPagina);
            return new StaticPagedList<EscalaNacionalViewModel>(result.Item1, pagina, tamanioPagina, result.Item2);
        }

        public int IngresarGrupoColegioApi(string codigoGrupo, string nombreGrupo, int consecutivoSedeJornada, int consecutivoGrado, int consecutivoPeriodo, string numeroIdentificacionCoordinador)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gru_codigo", codigoGrupo },
                { "gru_nombre", nombreGrupo },
                { "sedjor_consecutivoP", consecutivoSedeJornada },
                { "gra_consecutivoP", consecutivoGrado },
                { "gru_per_consecutivo", consecutivoPeriodo },
                { "gru_numero_identificacion_coordinador", numeroIdentificacionCoordinador }
            };

            try
            {
                return institucion.BaseDatos.ExecuteNonQuery("pa_apis_aca_grupos_ingresar", parametros);
            }
            catch (Exception ex)
            {
                throw new UserException(ex.Message);
            }
        }

        public async Task<IPagedList<GrupoViewModel>> ObtenerGruposColegioAPIAsync(int pagina, int totalPagina, int? consecutivoSedeJornada, int? consecutivoAnnoLectivo, int? consecutivoGrado)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "sedjor_consecutivoP", consecutivoSedeJornada },
                { "per_consecutivoP", consecutivoAnnoLectivo },
                { "gra_consecutivoP", consecutivoGrado }
            };

            var result = await institucion.BaseDatos.ExecuteQueryAsync<GrupoViewModel>("pa_apis_aca_grupos_colegios_retornar", parametros, pagina, totalPagina);
            return new StaticPagedList<GrupoViewModel>(result.Item1, pagina, totalPagina, result.Item2);
        }

        public async Task<GrupoViewModel> ObtenerDetalleGrupoApiAsync(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gru_consecutivoP", consecutivo }
            };
            return (await institucion.BaseDatos.ExecuteQueryAsync<GrupoViewModel>("pa_aca_apis_grupos_detalle_retornar", parametros)).FirstOrDefault();
        }

        #region Comfama
        public async Task<GrupoViewModel> ObtenerDetalleGrupoComfamaApiAsync(string codigoGrupo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gru_codigo", codigoGrupo }
            };
            return (await institucion.BaseDatos.ExecuteQueryAsync<GrupoViewModel>("paz_comfama_aca_grupos_disponibles_detalle_retornar", parametros)).FirstOrDefault();
        }

        #endregion Comfama

        #endregion
        #endregion API

        #endregion

        #region Estudiantes Grupo

        public IList<EstudianteGrupoViewModel> ObtenerEstudiantesGrupo(int grupo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gruren_consecutivoP", grupo }
            };
            return institucion.BaseDatos.ExecuteQuery<EstudianteGrupoViewModel>("pa_aca_grupos_renovacion_estudiantes_retornar", parametros);
        }

        public EstudianteGrupoViewModel ObtenerDetalleEstudianteGrupo(int grupo, string matricula)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gruren_consecutivoP", grupo },
                { "mat_codigoP", matricula }
            };
            return institucion.BaseDatos.ExecuteQuery<EstudianteGrupoViewModel>("pa_aca_grupos_renovacion_estudiantes_detalle_retornar", parametros).FirstOrDefault();
        }

        public void ModificarEstudianteGrupoCurso(int grupo, string matricula, int? cursoAnterior, int? cursoNuevo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gru_consecutivoP" , grupo },
                { "mat_codigoP" , matricula },
                { "plaaca_consecutivo_anterior", cursoAnterior },
                { "plaaca_consecutivo_nuevo", cursoNuevo },
                { "aud_usuario_sesion", per_codigoP },
                { "aud_ip",ClientIpAddress },
                { "aud_cat_codigo", CategoriaAuditoria },
                { "aud_usuario_q10", codigo_ejecutivo_institucion },
                { "aud_interno", false }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_matriculas_programas_trabajo_superior_cursos_trasladar", parametros);
        }

        public void ModificarObservacionEstudianteGrupo(int grupo, string matricula, string observacion)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gruren_consecutivoP", grupo },
                { "mat_codigoP", matricula },
                { "hmatpro_observacion_actas" , observacion }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_matriculas_programas_trabajo_observaciones_actas_modificar", parametros);
        }

        #endregion

        #region Renovaciones Grupos

        public List<RenovarGrupoViewModel> ObtenerRenovacionesGrupo(int grupo, bool? estado = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gru_estado", estado },
                { "gru_consecutivoP", grupo }
            };

            return institucion.BaseDatos.ExecuteQuery<RenovarGrupoViewModel>("pa_aca_grupos_renovacion_retornar", parametros).ToList();
        }

        public RenovarGrupoViewModel ObtenerDetalleRenovacionGrupo(int renovacion)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gruren_consecutivoP", renovacion }
            };

            return institucion.BaseDatos.ExecuteQuery<RenovarGrupoViewModel>("pa_aca_grupos_renovacion_detalle_retornar", parametros).FirstOrDefault();
        }

        public void RenovarGrupo(int grupo, string codigo, string nombre, int? programa, int? periodo, string persona, int? programaPensun, string nivel, DateTime? fechaInicio = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gruren_gru_consecutivo" , grupo },
                { "gruren_codigo" , codigo },
                { "gruren_nombre" , nombre },
                { "gruren_sedjorprogra_consecutivo" , programa },
                { "gruren_per_consecutivo" , periodo },
                { "gruren_per_codigo_coordinador" , persona },
                { "gruren_niv_codigo" , nivel },
                { "gruren_propen_consecutivo" , programaPensun },
                { "gruren_fecha_inicio",fechaInicio }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_grupos_renovacion_ingresar", parametros);
        }

        public bool ValidarRenovacionGrupo(int grupo, int? periodo, string nivel)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gruren_gru_consecutivo", grupo },
                { "gruren_per_consecutivo", periodo },
                { "gruren_niv_codigo", nivel },
            };

            return Convert.ToBoolean(institucion.BaseDatos.ExecuteNonQuery("pa_aca_grupos_renovacion_validar", parametros));
        }

        public IList<RenovarGrupoViewModel> ObtenerNivelesGrupo(int grupo, int periodo, bool? estado = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gru_consecutivoP", grupo },
                { "gru_estado", estado },
                { "per_consecutivoP", periodo }
            };

            return institucion.BaseDatos.ExecuteQuery<RenovarGrupoViewModel>("pa_aca_grupos_renovacion_niveles_retornar", parametros);
        }

        #endregion

        #region Asignaturas Grupo-Salud

        public IList<AsignaturaGrupoViewModel> ObtenerAsignaturasGrupo(int grupo, string asignatura, int? periodo = null, bool? cerrado = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gruasicur_gru_consecutivo", grupo },
                { "gruasicur_asi_codigo", asignatura },
                { "per_consecutivoP", periodo },
                { "plaaca_cerrado", cerrado },
            };

            return institucion.BaseDatos.ExecuteQuery<AsignaturaGrupoViewModel>("pa_aca_grupos_asignaturas_cursos_retornar", parametros);
        }

        public int IngresarAsignaturaGrupoEstudiantes(int grupo, string asignatura, int planAcademico)
        {
            string codigoAuditoria = Constantes.CATEGORIAS_AUDITORIA.First(c => c.Value == "Gestión académica").Key;

            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gruasicur_gru_consecutivo", grupo },
                { "gruasicur_asi_codigo", asignatura },
                { "gruasicur_plaaca_consecutivo", planAcademico },
                { "aud_usuario_sesion", per_codigoP },
                { "aud_ip", ClientIpAddress },
                { "aud_cat_codigo", codigoAuditoria },
                { "aud_usuario_q10", codigo_ejecutivo_institucion }
            };

            return institucion.BaseDatos.ExecuteNonQuery("pa_aca_grupos_asignaturas_cursos_estudiantes_ingresar", parametros);
        }

        public void IngresarAsignaturaGrupo(int grupo, string asignatura, int planAcademico)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gruasicur_gru_consecutivo", grupo },
                { "gruasicur_asi_codigo", asignatura },
                { "gruasicur_plaaca_consecutivo", planAcademico }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_grupos_asignaturas_cursos_ingresar", parametros);
        }

        public void EliminarAsignaturaGrupo(int grupoAsignatura)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gruasicur_consecutivoP", grupoAsignatura }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_grupos_asignaturas_cursos_eliminar", parametros);
        }

        #endregion

        #region Modulos Grupo-Salud

        public void IngresarModulosGrupoEstudiante(int grupo, string modulo, int planAcademico)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "grumodcur_gru_consecutivo", grupo },
                { "grumodcur_mod_codigo", modulo },
                { "grumodcur_plaaca_consecutivo", planAcademico }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_grupos_modulos_cursos_estudiantes_ingresar", parametros);
        }

        public IList<ModuloGrupoViewModel> ObtenerModulosGrupo(int idGrupo, string codigo, int? periodo = null, bool? cerrado = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "grumodcur_gru_consecutivo", idGrupo },
                { "grumodcur_mod_codigo", codigo },
                { "per_consecutivoP", periodo },
                { "plaaca_cerrado", cerrado },
            };

            return institucion.BaseDatos.ExecuteQuery<ModuloGrupoViewModel>("pa_aca_grupos_modulos_cursos_retornar", parametros);
        }

        public void IngresarModuloGrupo(int grupo, string modulo, int planAcademico)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "grumodcur_gru_consecutivo", grupo },
                { "grumodcur_mod_codigo", modulo },
                { "grumodcur_plaaca_consecutivo", planAcademico }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_grupos_modulos_cursos_ingresar", parametros);
        }

        public void EliminarModuloGrupo(int grupoModulo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "grumodcur_consecutivoP", grupoModulo }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_grupos_modulos_cursos_eliminar", parametros);
        }

        public void modificarModuloPredeterminado(int grupoModulo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "grumodcur_consecutivoP", grupoModulo }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_grupos_modulos_cursos_predeterminado_modificar", parametros);
        }
        #endregion

        #region Horarios Planes Academicos

        public IList<HorarioViewModel> ObtenerHorariosPersona(string codigoPersona, char rol, int? consecutivoAula = null, DateTime? fechaInicio = null, DateTime? fechaFin = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "per_codigoP", codigoPersona },
                { "rol_codigoP", rol },
                { "filtros_aula", consecutivoAula },
                { "filtros_fecha_inicio", fechaInicio },
                { "filtros_fecha_fin", fechaFin },
            };

            var informacion = institucion.BaseDatos.ExecuteQuery<HorarioViewModel>("pa_aca_horarios_personas_retornar", parametros);

            //Se realiza agrupacion por horario por que se consulta en un tabla detalle y puede que la información este duplicada
            if (informacion != null && informacion.Count() > 0)
            {
                informacion = informacion.GroupBy(m => m.hor_consecutivoP).Select(m => m.First()).ToList();
            }

            return informacion;
        }

        public IList<HorarioViewModel> ObtenerHorariosIndividuales(string codigoDocente = null, DateTime? fechaInicio = null, DateTime? fechaFin = null, string asignatura = null, string estudiante = null, int? periodo = null, char tipoAgrupacion = '1', string sede = null, int pageIndex = 1 , int pageSize = 12)
        {
            /*
             TipoAgrupacion 1 -> Agrupar por hor_consecutivoP
             TipoAgrupacion 2 -> Filtrar por parámetro asignatura igual a hor_consecutivoP y agrupar por hor_consecutivoP
             */

            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_docente", codigoDocente },
                { "filtros_fecha_inicio", fechaInicio },
                { "filtros_fecha_fin", fechaFin },
                { "filtros_asignatura", asignatura },
                { "filtros_estudiante", estudiante },
                { "filtros_periodo", periodo },
                { "sed_codigoP", sede }
            };

            var informacion = institucion.BaseDatos.ExecuteQuery<HorarioViewModel>("pa_aca_horarios_asignaturas_individuales_retornar", parametros, pageIndex, pageSize, out int totalCount);
            

            //Se realiza agrupacion por horario por que se consulta en un tabla detalle y puede que la información este duplicada
            if (informacion != null && informacion.Count() > 0)
            {
                if (tipoAgrupacion == '2')
                {
                    informacion = informacion.Where(m => m.horasi_asi_codigo == asignatura).ToList();
                }

                informacion = informacion.GroupBy(m => m.hor_consecutivoP).Select(m => m.First()).ToList();
            }

            return informacion;
        }

        public IList<HorarioViewModel> ObtenerHorarios(int? consecutivoPlan, string codigoDocente = null, int? consecutivoAula = null, DateTime? fechaInicio = null, DateTime? fechaFin = null, string asignatura = null, string estudiante = null, int? periodo = null, int? repeticion = null, bool cursoArchivado = false, int? matriculaPlan = null, int? gradoPeriodo = null, char TipoAgrupacion = '1', string sede = null)
        {
            /*
             TipoAgrupacion 1 -> Agrupar por hor_consecutivoP
             TipoAgrupacion 2 -> Filtrar por parámetro asignatura igual a hor_consecutivoP y agrupar por hor_consecutivoP
             */
            IList<HorarioViewModel> informacion;

            var parametros = new Dictionary<string, object>() {
                    { "aplent_codigoP", institucion.aplent_codigoP },
                    { "filtros_docente", codigoDocente },
                    { "filtros_fecha_inicio", fechaInicio },
                    { "filtros_fecha_fin", fechaFin },
                    { "filtros_asignatura", asignatura },
                    { "filtros_estudiante", estudiante },
                    { "filtros_periodo", periodo },
                    { "sed_codigoP", sede }
                };

            //Lógica para asignaturas individuales
            if (asignatura != null)
            {
                informacion = institucion.BaseDatos.ExecuteQuery<HorarioViewModel>("pa_aca_horarios_asignaturas_individuales_retornar", parametros);
            }
            else
            {
                parametros.Add("filtros_plan_academico", consecutivoPlan);
                parametros.Add("filtros_aula", consecutivoAula);
                parametros.Add("filtros_repeticion", repeticion);
                parametros.Add("incluir_cursos_archivados", cursoArchivado);
                parametros.Add("matpla_consecutivoP", matriculaPlan);
                parametros.Add("graper_consecutivoP", gradoPeriodo);

                informacion = institucion.BaseDatos.ExecuteQuery<HorarioViewModel>("pa_aca_horarios_retornar", parametros);
            }

            //Se realiza agrupacion por horario por que se consulta en un tabla detalle y puede que la información este duplicada
            if (informacion != null && informacion.Count() > 0)
            {
                if (TipoAgrupacion == '2' && !asignatura.IsNullOrEmpty())
                {
                    informacion = informacion.Where(m => m.horasi_asi_codigo == asignatura).ToList();
                }

                informacion = informacion.GroupBy(m => m.hor_consecutivoP).Select(m => m.First()).ToList();

            }

            return informacion;
        }

        public IList<HorarioViewModel> ObtenerHorariosIndividualesOpciones(string asignatura = null, string codigoDocente = null, int? periodo = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_docente", codigoDocente },
                { "filtros_asignatura", asignatura },
                { "filtros_periodo", periodo }
            };

            return institucion.BaseDatos.ExecuteQuery<HorarioViewModel>("pa_aca_horarios_asignaturas_lista_retornar", parametros);
        }

        public bool ExisteAulaVirtualHorarioIndividual(string asignatura = null, int? periodo = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_asignatura", asignatura },
                { "filtros_periodo", periodo }
            };

            return Convert.ToBoolean(institucion.BaseDatos.ExecuteNonQuery("pa_aca_horarios_asignaturas_aula_virtual_validar", parametros));
        }

        public async Task<IPagedList<HorarioViewModel>> ObtenerHorariosColegiosAPIAsync(int? consecutivoGrado = null, int? consecutivoGrupo = null, string codigoAsignatura = null, int? consecutivoAnioLectivo = null, DateTime? fechaInicio = null, DateTime? fechaFin = null, int? consecutivoAulaFisica = null, string codigoAulaVirtual = null, bool? incluirCursosArchivados = false, int pagina = 1, int tamanioPagina = Constantes.PAGINACION_TAMANO_PAGINA)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "hor_consecutivoP", null },
                { "filtros_grado", consecutivoGrado },
                { "filtros_grupo", consecutivoGrupo },
                { "filtros_asignatura ", codigoAsignatura },
                { "filtros_periodo", consecutivoAnioLectivo },
                { "filtros_fecha_inicio", fechaInicio },
                { "filtros_fecha_fin", fechaFin },
                { "filtros_aula_fisica", consecutivoAulaFisica },
                { "filtros_aula_virtual", codigoAulaVirtual },
                {"incluir_cursos_archivados",incluirCursosArchivados }
            };

            var result = await institucion.BaseDatos.ExecuteQueryAsync<HorarioViewModel>("pa_apis_aca_horarios_colegios_retornar", parametros, pagina, tamanioPagina);
            return new StaticPagedList<HorarioViewModel>(result.Item1, pagina, tamanioPagina, result.Item2);
        }

        public async Task<HorarioViewModel> ObtenerDetalleHorarioColegiosAPIAsync(int consecutivoHorario)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "hor_consecutivoP", consecutivoHorario },
            };

            return (await institucion.BaseDatos.ExecuteQueryAsync<HorarioViewModel>("pa_apis_aca_horarios_colegios_retornar", parametros)).FirstOrDefault();
        }

        //Se valida si cuenta con horarios repetidos, de ser así retorna true, sino false
        public bool ValidarHorarios(int repeticion)
        {

            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_repeticion", repeticion },
            };

            return Convert.ToBoolean(institucion.BaseDatos.ExecuteNonQuery("pa_aca_horarios_existentes_validar", parametros));

        }

        public async Task<IPagedList<HorarioViewModel>> ObtenerHorariosCompetenciasAPIAsync(DateTime? fechaInicio, DateTime? fechaFin, bool? incluirCursosArchivados, int? consecutivoCurso = null, string identificacionDocente = null, int? consecutivoAulaFisica = null, string codigoAulaVirtual = null, int pagina = 1, int tamanioPagina = Constantes.PAGINACION_TAMANO_PAGINA)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_fecha_inicio", fechaInicio },
                { "filtros_fecha_fin", fechaFin },
                { "filtros_plan_academico", consecutivoCurso },
                { "filtros_docente", identificacionDocente },
                { "filtros_aula_fisica", consecutivoAulaFisica },
                { "filtros_aula_virtual", codigoAulaVirtual },
                { "incluir_cursos_archivados", incluirCursosArchivados}
            };

            var result = await institucion.BaseDatos.ExecuteQueryAsync<HorarioViewModel>("pa_apis_aca_horarios_competencias_retornar", parametros, pagina, tamanioPagina);
            return new StaticPagedList<HorarioViewModel>(result.Item1, pagina, tamanioPagina, result.Item2);
        }

        public async Task<HorarioViewModel> ObtenerDetalleHorarioCompetenciasAPIAsync(int consecutivoHorario)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "hor_consecutivo", consecutivoHorario }
            };

            return (await institucion.BaseDatos.ExecuteQueryAsync<HorarioViewModel>("pa_apis_aca_horarios_competencias_retornar", parametros)).FirstOrDefault();
        }

        public async Task<IList<HorarioViewModel>> ObtenerHorariosAPIAsync(int? consecutivoPlan, string docente = null, int? consecutivoAula = null, DateTime? fechaInicio = null, DateTime? fechaFin = null, string asignatura = null, string estudiante = null, int? periodo = null, int? repeticion = null, bool cursoArchivado = false, int? matriculaPlan = null, int? gradoPeriodo = null, char TipoAgrupacion = '1', string sede = null, char? tipoAsignaturas = null)
        {
            /*
             TipoAgrupacion 1 -> GrouBy hor_consecutivoP
             TipoAgrupacion 2 -> where asignatura and GrouBy hor_consecutivoP
             */

            //TODO: se deben filtrar por asignatura
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_docente", docente },
                { "filtros_plan_academico", consecutivoPlan },
                { "filtros_aula", consecutivoAula },
                { "filtros_fecha_inicio", fechaInicio },
                { "filtros_fecha_fin", fechaFin },
                { "filtros_asignatura", asignatura },
                { "filtros_estudiante", estudiante },
                { "filtros_periodo", periodo },
                { "filtros_repeticion", repeticion },
                { "incluir_cursos_archivados", cursoArchivado },
                { "matpla_consecutivoP", matriculaPlan },
                { "graper_consecutivoP", gradoPeriodo },
                { "sed_codigoP", sede },
                { "asi_tipo_evaluacion", tipoAsignaturas }
            };

            var informacion = await institucion.BaseDatos.ExecuteQueryAsync<HorarioViewModel>("pa_aca_horarios_por_tipo_retornar", parametros);

            //Se realiza agrupacion por horario por que se consulta en un tabla detalle y puede que la información este duplicada
            if (informacion != null && informacion.Count() > 0)
            {
                if (TipoAgrupacion == '2' && !asignatura.IsNullOrEmpty())
                {
                    informacion = informacion.Where(m => m.horasi_asi_codigo == asignatura).ToList();
                }

                informacion = informacion.GroupBy(m => m.hor_consecutivoP).Select(m => m.First()).ToList();

            }

            return informacion;
        }

        public IList<HorarioViewModel> ObtenerHorariosPrograma(string docentes, string asignaturas, string programa, string nivel, DateTime? fechaInicio = null, DateTime? fechaFin = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "pro_codigoP", programa },
                { "niv_codigoP", nivel },
                { "per_codigo_docente", docentes },
                { "asi_codigoP", asignaturas },
                { "filtros_fecha_inicio", fechaInicio },
                { "filtros_fecha_fin", fechaFin }
            };

            var informacion = institucion.BaseDatos.ExecuteQuery<HorarioViewModel>("pa_aca_horarios_programas_retornar", parametros);

            return informacion;
        }

        public List<MesViewModel> ObtenerMesesHorarios(string asignatura, int? periodo, string docente = null, DateTime? fechaInicio = null, DateTime? fechaFin = null)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "asi_codigoP", asignatura },
                { "per_consecutivoP", periodo },
                { "per_codigoP", docente },
                { "fecha_inicio", fechaInicio },
                { "fecha_fin", fechaFin },
            };

            return institucion.BaseDatos.ExecuteQuery<MesViewModel>("pa_aca_horarios_en_meses_retornar", parametros).ToList();
        }

        public HorarioViewModel ObtenerDetalleHorario(int? consecutivoHorario, string codigoIntegracion = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "hor_consecutivoP", consecutivoHorario },
                { "hor_codigo_integracion", codigoIntegracion }
            };

            return institucion.BaseDatos.ExecuteQuery<HorarioViewModel>("pa_aca_horarios_detalle_retornar", parametros).FirstOrDefault();
        }

        public async Task<HorarioViewModel> ObtenerDetalleHorarioAsync(int? consecutivoHorario, string codigoIntegracion = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "hor_consecutivoP", consecutivoHorario },
                { "hor_codigo_integracion", codigoIntegracion }
            };

            return (await institucion.BaseDatos.ExecuteQueryAsync<HorarioViewModel>("pa_aca_horarios_detalle_retornar", parametros)).FirstOrDefault();
        }

        public IList<HorarioViewModel> ObtenerHorariosNoEditables(int codigoRepeticion, DateTime? fechaDesde)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "rephor_consecutivoP", codigoRepeticion },
                { "filtros_fechaDesde", fechaDesde }
            };

            return institucion.BaseDatos.ExecuteQuery<HorarioViewModel>("pa_aca_repeticiones_horarios_validar_para_modificar", parametros);
        }

        #region Asignaturas Individuales

        public async Task<IPagedList<HorarioViewModel>> ObtenerHorariosAsignaturasIndividualesAPIAsync(DateTime fechaInicio, DateTime fechaFin, int tamanioPagina, int pageIndex, string codigoAsignatura = null, string identificacionDocente = null, int? consecutivoAula = null, string codigoAula = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_fecha_inicio", fechaInicio },
                { "filtros_fecha_fin", fechaFin },
                { "asi_codigo", codigoAsignatura },
                { "filtros_docente", identificacionDocente },
                { "filtros_aula_fisica", consecutivoAula },
                { "filtros_aula_virtual", codigoAula }
            };

            var result = await institucion.BaseDatos.ExecuteQueryAsync<HorarioViewModel>("pa_apis_aca_horarios_asignaturas_retornar", parametros, pageIndex, tamanioPagina);
            return new StaticPagedList<HorarioViewModel>(result.Item1, pageIndex, tamanioPagina, result.Item2);
        }

        public async Task<HorarioViewModel> ObtenerDetalleHorariosAsignaturasIndividualesAPIAsync(int? consecutivoHorario)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "hor_consecutivoP", consecutivoHorario }
            };

            return (await institucion.BaseDatos.ExecuteQueryAsync<HorarioViewModel>("pa_apis_aca_horarios_asignaturas_retornar", parametros)).FirstOrDefault();
        }

        #endregion Asignaturas Individuales

        public async Task<int> IngresarHorario(int? consecutivoPlan, int? consecutivoAula, DateTime fechaInicio, DateTime fechaFinal, int? consecutivoRepeticion, string docente = null, int? periodo = null, int? gradoPeriodo = null, string titulo = null, string observacion = null, char origenCreacion = (char)ProvidersCalendarWebHook.Q10, bool preventSync = false, bool forceSend = false, int? cupoMaximo = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "hor_plaaca_consecutivo", consecutivoPlan },
                { "hor_aul_consecutivo", consecutivoAula },
                { "hor_fecha_inicio", fechaInicio },
                { "hor_fecha_fin", fechaFinal },
                { "hor_rephor_consecutivo", consecutivoRepeticion },
                { "hor_per_codigo_docente", docente },
                { "hor_per_consecutivo", periodo },
                { "hor_graper_consecutivo", gradoPeriodo },
                { "hor_titulo", titulo },
                { "hor_observacion", observacion },
                { "hor_origen", origenCreacion },
                { "hor_cupo_maximo", cupoMaximo }
            };

            var idHorario = institucion.BaseDatos.ExecuteNonQuery("pa_aca_horarios_ingresar", parametros);

            if (!docente.IsNullOrEmpty() && institucion.ValidarPersonalizacion(Constantes.ZPERSONALIZACION_SUGERENCIAS_AZURE_SEARCH))
            {
                BackgroundJob.Enqueue(() => new ServicioSuggestions(institucion, usuario).ActualizarAccesoDocentesAdministrativosJob(institucion.aplent_id, docente, "D", null, null));
            }

            //Validamos que el cambio se haya hecho en Q10 y no sea en ejecución de los webhooks
            if (origenCreacion == (char)ProvidersCalendarWebHook.Q10 && (!preventSync) && institucion.ValidarPersonalizacion(Constantes.ZINTEGRACION_CALENDAR_OUTLOOK))
            {
                await new ServicioAcademica(institucion, usuario).CrearActualizarEventoOutlook(idHorario);
            }

            if (institucion.ValidarPersonalizacion(Constantes.ZPERSONALIZACION_INTERACTUAR))
            {
                if (origenCreacion == (char)ProvidersCalendarWebHook.Q10
                    && consecutivoAula.HasValue
                    && !(!titulo.IsNullOrEmpty() && !consecutivoPlan.HasValue))
                {
                    var detalleAula = Get<ServicioEstablecimiento>(institucion, usuario).ObtenerDetalleAula(consecutivoAula.Value);
                    if (!detalleAula.aul_email.IsNullOrEmpty())
                    {
                        ModificarEstadoAprobacionHorario(idHorario, 'P');
                    }
                }

                if (fechaInicio.Date == DateTime.Today.AddDays(1) && fechaInicio > DateTime.Now.AddDays(1).AddMinutes(-1))
                {
                    BackgroundJob.Schedule(() => new ServicioAcademica(null, null).SendNotificationsMails(null, institucion.aplent_id, idHorario), fechaInicio.AddDays(-1));
                }
            }

            return idHorario;
        }

        public void IngresarAuditoriaHorario(DateTime fechaInicio, DateTime fechaFin, TimeSpan? horaInicio, TimeSpan? horaFin, string nombrePlanCurso, string tipoAula, bool repetir, string[] diasRepeticion = null, bool? excluirFestivos = false, bool? esAsignatura = false, bool? esCurso = false)
        {
            fechaFin = repetir ? fechaFin.Add(horaFin.Value) : fechaInicio.Add(horaFin.Value);
            fechaInicio = fechaInicio.Add(horaInicio.Value);

            string fechaInicioFormateada = fechaInicio.ToString("dd/MM/yyyy");// Formato de fecha DD/MM/AA
            string fechaFinFormateada = fechaFin.ToString("dd/MM/yyyy");
            string horaInicioFormateada = fechaInicio.ToString("hh:mm tt"); // Formato de hora hh:mm tt
            string horaFinFormateada = fechaFin.ToString("hh:mm tt");
            var mensajeAuditoria = "";

            var planAcademico = (esCurso ?? false) ? "del curso" : "del plan académico";

            if (!institucion.EsColegio) planAcademico = (esAsignatura ?? false) ? institucion.CustomText("de la asignatura") + ":" : "del curso";

            if (repetir)
            {
                string[] dias = { "domingo", "lunes", "martes", "miércoles", "jueves", "viernes", "sábado" };

                string diasHorario = string.Join(", ", diasRepeticion.Select(m => dias[Convert.ToInt32(m)]));

                mensajeAuditoria = $"Se creó el horario de aula {tipoAula} {planAcademico} {nombrePlanCurso} con hora de inicio {horaInicioFormateada} y hora fin {horaFinFormateada} con repetición los días {diasHorario}, desde {fechaInicioFormateada} hasta {fechaFinFormateada}, {(excluirFestivos ?? false ? "excluyendo" : "incluyendo")} días no académicos.";
            }
            else
            {
                mensajeAuditoria = $"Se creó el horario de aula {tipoAula} {planAcademico} {nombrePlanCurso} con fecha de inicio {fechaInicioFormateada} {horaInicioFormateada} y fecha fin {fechaFinFormateada} {horaFinFormateada}";
            }

            // Se envía la categoría gestión académica de código "04" y el sp que ingresa los horarios 
            Get<ServicioSeguridad>().IngresarAuditoria(mensajeAuditoria, "04", "pa_aca_horarios_ingresar");
        }

        public async Task ModificarHorario(int consecutivoHorario, int? consecutivoAula, DateTime fechaInicio, DateTime fechaFinal, int? consecutivoRepeticion, string docente, int? periodo = null, string titulo = null, string observacion = null, char origenModificacion = (char)ProvidersCalendarWebHook.Q10, int? cupoMaximo = null, bool? semanalModificadoDetallado = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "hor_consecutivoP", consecutivoHorario },
                { "hor_aul_consecutivo", consecutivoAula },
                { "hor_fecha_inicio", fechaInicio },
                { "hor_fecha_fin", fechaFinal },
                { "hor_rephor_consecutivo", consecutivoRepeticion },
                { "hor_per_codigo_docente",docente },
                { "hor_per_consecutivo",periodo },
                { "hor_titulo", titulo },
                { "hor_observacion", observacion },
                { "aud_usuario_sesion", per_codigoP },
                { "aud_ip",ClientIpAddress },
                { "aud_cat_codigo", CategoriaAuditoria },
                { "aud_usuario_q10", codigo_ejecutivo_institucion },
                { "aud_interno", false },
                { "hor_cupo_maximo", cupoMaximo },
                { "horsem_modificado_detallado", semanalModificadoDetallado }
            };

            var detalleHorario = new HorarioViewModel();

            bool aplicaSugerenciasAzureSearch = institucion.ValidarPersonalizacion(Constantes.ZPERSONALIZACION_SUGERENCIAS_AZURE_SEARCH);
            bool poseePersonalizacionInteractuar = institucion.ValidarPersonalizacion(Constantes.ZPERSONALIZACION_INTERACTUAR);

            // Variables para la actualización de sugerencias de docentes anteriores.
            string docenteAnterior = string.Empty;
            List<SuggestionViewModel> estudiantesDocenteAnterior = null;

            // Si el horario ya no tiene docente, antes de modificar, verificamos si anteriormente si tenía y lo quitaron, ya que se deben
            // de actualizar las sugerencias para que no pueda buscar personas que no le aplican.
            if ((docente.IsNullOrEmpty() && aplicaSugerenciasAzureSearch) || poseePersonalizacionInteractuar)
            {
                detalleHorario = ObtenerDetalleHorario(consecutivoHorario);

                if (aplicaSugerenciasAzureSearch && !detalleHorario.hor_per_codigo_docente.IsNullOrEmpty())
                {
                    docenteAnterior = detalleHorario.hor_per_codigo_docente;
                    // Se consultan las sugerencias antes de modificar el horario para obtener los estudiantes afectados del horario
                    // y posteriormente, en ActualizarAccesoDocentesAdministrativosJob se elimine la búsqueda si ya no se posee relación.
                    estudiantesDocenteAnterior = await new ServicioSuggestions(institucion, usuario).ObtenerAccesoPersonas(docenteAnterior, "D");
                }
            }

            if (poseePersonalizacionInteractuar)
            {
                if (detalleHorario.hor_aul_consecutivo != consecutivoAula)
                {
                    await new ServicioAcademica(institucion, usuario).EliminarEventoOutlook(consecutivoHorario);
                    ModificarIntegracionHorario(consecutivoHorario, null, null);
                }
            }

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_horarios_modificar", parametros);

            if ((!docente.IsNullOrEmpty() || !docenteAnterior.IsNullOrEmpty()) && aplicaSugerenciasAzureSearch)
            {
                BackgroundJob.Enqueue(() => new ServicioSuggestions(institucion, usuario).ActualizarAccesoDocentesAdministrativosJob(institucion.aplent_id, docente.IsNullOrEmptyThen(docenteAnterior), "D", null, estudiantesDocenteAnterior));
            }

            //Validamos que el cambio se haya hecho en Q10 y no sea en ejecución de los webhooks
            if (origenModificacion == (char)ProvidersCalendarWebHook.Q10 && institucion.ValidarPersonalizacion(Constantes.ZINTEGRACION_CALENDAR_OUTLOOK))
            {
                await new ServicioAcademica(institucion, usuario).CrearActualizarEventoOutlook(consecutivoHorario);
            }

            if (institucion.ValidarPersonalizacion(Constantes.ZPERSONALIZACION_INTERACTUAR))
            {
                int? consecutivoPlan = detalleHorario?.hor_plaaca_consecutivo;

                if (origenModificacion == (char)ProvidersCalendarWebHook.Q10
                    && consecutivoAula.HasValue
                    && !(!titulo.IsNullOrEmpty() && !consecutivoPlan.HasValue))
                {
                    var detalleAula = Get<ServicioEstablecimiento>(institucion, usuario).ObtenerDetalleAula(consecutivoAula.Value);

                    if (!detalleAula.aul_email.IsNullOrEmpty())
                    {
                        ModificarEstadoAprobacionHorario(consecutivoHorario, 'P');
                    }
                }

                if (fechaInicio.Date == institucion.FechaHoraActual.Date.AddDays(1) && fechaInicio > institucion.FechaHoraActual.AddDays(1).AddMinutes(-1))
                {
                    BackgroundJob.Schedule(() => new ServicioAcademica(null, null).SendNotificationsMails(null, institucion.aplent_id, consecutivoHorario), fechaInicio.AddDays(-1));
                }
            }
        }

        public void ModificarEstadoAprobacionHorario(int consecutivoHorario, char? estadoAprobacion)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "hor_consecutivoP", consecutivoHorario },
                { "hor_estado_aprobacion", estadoAprobacion }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_horarios_estado_aprobacion_modificar", parametros);

        }

        public void ModificarIntegracionHorario(int consecutivoHorario, string codigoIntegracion, char? plataformaIntegracion)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "hor_consecutivoP", consecutivoHorario },
                { "hor_codigo_integracion", codigoIntegracion },
                { "hor_plataforma_integracion", plataformaIntegracion }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_horarios_informacion_integracion_modificar", parametros);
        }

        public async Task EliminarHorario(int consecutivoHorario, char? origenEliminacion = (char)ProvidersCalendarWebHook.Q10, bool semanalModificadoDetallado = false, bool? guardarAuditoria = true)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "hor_consecutivoP", consecutivoHorario },
                { "hor_rephor_consecutivo", null },
                { "aud_usuario_sesion", per_codigoP },
                { "aud_ip",ClientIpAddress },
                { "aud_cat_codigo", CategoriaAuditoria },
                { "aud_usuario_q10", codigo_ejecutivo_institucion },
                { "aud_interno", false },
                { "horsem_modificado_detallado", semanalModificadoDetallado },
                { "guardar_auditoria", guardarAuditoria }
            };

            //Validamos que el cambio se haya hecho en Q10 y no sea en ejecución de los webhooks
            if (origenEliminacion == (char)ProvidersCalendarWebHook.Q10 && institucion.ValidarPersonalizacion(Constantes.ZINTEGRACION_CALENDAR_OUTLOOK))
            {
                await new ServicioAcademica(institucion, usuario).EliminarEventoOutlook(consecutivoHorario);
            }

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_horarios_eliminar", parametros);

        }

        public void EliminarHorariosRepeticion(int consecutivoRepeticion)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "hor_consecutivoP", null },
                { "hor_rephor_consecutivo", consecutivoRepeticion },
                { "aud_usuario_sesion", per_codigoP },
                { "aud_ip",ClientIpAddress },
                { "aud_cat_codigo", CategoriaAuditoria }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_horarios_eliminar", parametros);
        }

        public void EliminarRepeticion(int consecutivoRepeticion)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "rephor_consecutivoP", consecutivoRepeticion}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_repeticiones_horarios_eliminar", parametros);
        }

        public int IngresarRepeticionHorario(string dias, DateTime fechaInicio, DateTime fechaFinal, bool excluirFestivos = false)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "rephor_dias", dias },
                { "rephor_fecha_inicio", fechaInicio },
                { "rephor_fecha_fin", fechaFinal },
                { "rephor_festivo", excluirFestivos },
            };

            return institucion.BaseDatos.ExecuteNonQuery("pa_aca_repeticiones_horarios_ingresar", parametros);
        }

        public void ModificarRepeticionHorario(int codigo, string dias, DateTime fechaInicio, DateTime fechaFinal, bool excluirFestivos = false)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "rephor_consecutivoP", codigo },
                { "rephor_dias", dias },
                { "rephor_fecha_inicio", fechaInicio },
                { "rephor_fecha_fin", fechaFinal },
                { "rephor_festivo", excluirFestivos },
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_repeticiones_horarios_modificar", parametros);
        }

        public bool ExisteConflictoHorario(int? codigoHorario, int? codigoAula, string codigoDocente, DateTime? fechaInicio, DateTime? fechaFin, int? repeticion)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "hor_consecutivoP", codigoHorario },
                { "hor_aul_consecutivo", codigoAula },
                { "plaaca_per_codigo_docente", codigoDocente },
                { "hor_fecha_inicio", fechaInicio },
                { "hor_fecha_fin", fechaFin },
                { "hor_rephor_consecutivo", repeticion}//Este campo se envia cuando se van a modificar masivamente las repeticiones del horario
            };
            return Convert.ToBoolean(institucion.BaseDatos.ExecuteNonQuery("pa_aca_horarios_conflicto_revisar", parametros));
        }

        public bool ExisteConflictoHorarioRepeticion(int? codigoHorario, int? codigoAula, string codigoDocente, DateTime fechaInicio, DateTime fechaFin, DateTime? repetirHasta, string codigoPersona, int? consecutivoRepeticion, string diasRepeticion)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "hor_consecutivoP", codigoHorario },
                { "hor_aul_consecutivo", codigoAula },
                { "plaaca_per_codigo_docente", codigoDocente },
                { "hor_fecha_inicio", fechaInicio },
                { "hor_fecha_fin", fechaFin },
                { "repetir_hasta", repetirHasta },
                { "mat_per_codigo", codigoPersona },
                { "hor_rephor_consecutivo", consecutivoRepeticion },
                { "dias_repetición", diasRepeticion },
            };
            return Convert.ToBoolean(institucion.BaseDatos.ExecuteNonQuery("pa_aca_repeticiones_horarios_conflicto_revisar", parametros));
        }

        public List<CruceHorarioEstudianteViewModel> ExisteConflictoHorarioEstudiantes(int planAcademico, DateTime fechaInicio, DateTime fechaFin, TimeSpan horaInicio, TimeSpan horaFin, string dia = null, int? consecutivoHorario = null, int? consecutivoRepeticion = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "hor_consecutivoP", consecutivoHorario },
                { "plaaca_consecutivoP", planAcademico },
                { "hor_fecha_inicio", fechaInicio },
                { "hor_fecha_fin", fechaFin },
                { "hor_hora_inicio", horaInicio },
                { "hor_hora_fin", horaFin },
                { "hor_rephor_consecutivo", consecutivoRepeticion },
                { "hor_dia", dia }
            };
            return institucion.BaseDatos.ExecuteQuery<CruceHorarioEstudianteViewModel>("pa_aca_horarios_conflicto_estudiantes_validar", parametros).ToList();
        }

        public List<CruceHorarioEstudianteViewModel> ExisteConflictoHorarioEstudiantesIndividual(int? codigoHorario, DateTime fechaInicio, DateTime fechaFin, string persona)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "hor_consecutivoP", codigoHorario },
                { "hor_fecha_inicio", fechaInicio },
                { "hor_fecha_fin", fechaFin },
                { "per_codigoP", persona },
            };
            return institucion.BaseDatos.ExecuteQuery<CruceHorarioEstudianteViewModel>("pa_aca_horarios_asignatura_conflicto_estudiantes_validar", parametros).ToList();
        }

        public async Task<List<CruceHorarioEstudianteViewModel>> ExisteConflictoHorarioEstudiantesIndividualAsync(int? codigoHorario, DateTime fechaInicio, DateTime fechaFin, string persona)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "hor_consecutivoP", codigoHorario },
                { "hor_fecha_inicio", fechaInicio },
                { "hor_fecha_fin", fechaFin },
                { "per_codigoP", persona },
            };
            return (await institucion.BaseDatos.ExecuteQueryAsync<CruceHorarioEstudianteViewModel>("pa_aca_horarios_asignatura_conflicto_estudiantes_validar", parametros)).ToList();
        }

        public void TrasladarHorario(int planAcademico, string docenteAnterior, string docenteNuevo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaaca_consecutivoP", planAcademico },
                { "hor_per_codigo_docente", docenteAnterior },
                { "hor_per_codigo_docente_nuevo", docenteNuevo },
                { "aud_usuario_sesion", per_codigoP },
                { "aud_ip",ClientIpAddress },
                { "aud_cat_codigo", CategoriaAuditoria }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_horarios_docente_modificar", parametros);
        }

        public bool ExisteConflictoHorarioDocente(int planAcademico, string docenteAnterior, string docenteNuevo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaaca_consecutivoP", planAcademico },
                { "docente_anterior", docenteAnterior },
                { "docente_nuevo", docenteNuevo },
            };
            return Convert.ToBoolean(institucion.BaseDatos.ExecuteNonQuery("pa_aca_horarios_conflicto_docente_revisar", parametros));
        }

        public bool PuedeCrearHorarioEstudiante(int aula)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "aul_consecutivoP", aula },
                { "per_codigoP", usuario.per_codigoP },
            };

            return Convert.ToBoolean(institucion.BaseDatos.ExecuteNonQuery("paz_escuela_audio_sonido_aca_aula_asignatura_validar", parametros));
        }

        /// <summary>
        /// Esto método se encarga de validar el cruce de horarios entre aulas, docentes y cursos (Este metodo solo se encarga de validar UN (1) horario NO VALIDA REPETICIONES)
        /// </summary>
        /// <param name="filtro">
        /// Filtro a utilizar para validar horarios.
        /// <list type="bullet">
        /// <item>
        /// <description> 1 solo se requiere los parametros: planAcademico, codigoDocente. </description>
        /// </item>
        /// <item>
        /// <description> 2 solo se requiere los parametros: horario, aula, codigoDocente, fechaInicio, fechaFin, repeticionHorario. </description>
        /// </item>
        /// <item>
        /// <description> 3 solo se requiere los parametros: planAcademico, docenteAnterior, docenteNuevo. </description>
        /// </item>
        /// <item>
        /// <description> 4 solo se requiere los parametros: planAcademico, fechaInicio, fechaFin. </description>
        /// </item>
        /// <item>
        /// <description> 5 solo se requiere los parametros: horarioAulaVirtual, aulaVirtual, codigoDocente, fechaInicio, fechaFin. </description>
        /// </item>
        /// </list>
        /// </param>  
        /// <returns>
        /// Si hay cruce de horario en el filtro 1 se llenan propiedades dentro del modelo que se recibe, de tipo string denominada "nombre_asignatura" y "nombre_modulo"
        /// Si hay cruce de horario en los filtros 2,3,4,5 dentro del modelo que se recibe se llena la propiedad  de tipo boolen denominada "cruce"
        /// </returns>

        public CruceHorarioViewModel ValidarCruceHorario(int filtro, int? planAcademico = null, string codigoDocente = null, int? horario = null, int? aula = null, DateTime? fechaInicio = null, DateTime? fechaFin = null, int? repeticionHorario = null, string filtroEstudiante = null, string docenteAnterior = null, string docenteNuevo = null, string aulaVirtual = null, int? horarioAulaVirtual = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaaca_consecutivoP", planAcademico },
                { "plaaca_per_codigo_docente", codigoDocente },
                { "hor_consecutivoP", horario },
                { "hor_aul_consecutivo", aula },
                { "hor_fecha_inicio", fechaInicio },
                { "hor_fecha_fin", fechaFin },
                { "hor_rephor_consecutivo", repeticionHorario },
                { "filtro_estudiante", filtroEstudiante },
                { "docente_anterior", docenteAnterior },
                { "docente_nuevo", docenteNuevo },
                { "horaulvirt_aulvir_codigo", aulaVirtual },
                { "horaulvirt_consecutivoP", horarioAulaVirtual },
                { "filtro", filtro }
            };

            return institucion.BaseDatos.ExecuteQuery<CruceHorarioViewModel>("pa_aca_cruces_horarios_validar", parametros).FirstOrDefault();
        }

        /// <summary>
        /// Esta sobrecarga se utiliza para validar los horarios semanales y con repetición (Detallado), valida por docente y por aula
        /// </summary>
        public CruceHorarioViewModel ValidarCruceHorario(int? planAcademico = null, DateTime? fechaInicio = null, DateTime? fechaFin = null, string aulaVirtual = null, TimeSpan? horaInicio = null, TimeSpan? horaFin = null, string docente = null, string dia = null, int? aulaFisica = null, int? consecutivoHorarioSemanal = null, int? horarioVirtualConsecutivo = null, int? repeticionHorario = null, int? horario = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtro", 6 },
                { "hor_consecutivoP", horario },
                { "plaaca_consecutivoP", planAcademico },
                { "hor_fecha_inicio", fechaInicio },
                { "hor_fecha_fin", fechaFin },
                { "hor_aul_consecutivo", aulaFisica },
                { "horaulvirt_aulvir_codigo", aulaVirtual },
                { "horsem_consecutivoP", consecutivoHorarioSemanal },
                { "hor_rephor_consecutivo", repeticionHorario },
                { "hor_hora_inicio", horaInicio },
                { "hor_hora_fin", horaFin },
                { "plaaca_per_codigo_docente", docente },
                { "hor_dia", dia },
                { "horaulvirt_consecutivoP", horarioVirtualConsecutivo},
            };

            return institucion.BaseDatos.ExecuteQuery<CruceHorarioViewModel>("pa_aca_cruces_horarios_validar", parametros).FirstOrDefault();
        }

        #region Horarios Docentes Apoyo

        public IList<HorarioDocenteViewModel> ObtenerHorariosDocentes(int horario)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "hordoc_hor_consecutivo", horario },
            };

            return institucion.BaseDatos.ExecuteQuery<HorarioDocenteViewModel>("pa_aca_horarios_docentes_retornar", parametros);
        }

        public void IngresarHorarioDocentes(int horario, string docente, bool actualizarSugerencias = true)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "hordoc_hor_consecutivo", horario },
                { "hordoc_per_codigo", docente },
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_horarios_docentes_ingresar", parametros);

            if (actualizarSugerencias && !docente.IsNullOrEmpty() && institucion.ValidarPersonalizacion(Constantes.ZPERSONALIZACION_SUGERENCIAS_AZURE_SEARCH))
            {
                BackgroundJob.Enqueue(() => new ServicioSuggestions(institucion, usuario).ActualizarAccesoDocentesAdministrativosJob(institucion.aplent_id, docente, "D", null, null));
            }
        }

        public void EliminarHorariosDocentes(int horario, string docente)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "hordoc_hor_consecutivo", horario },
                { "hordoc_per_codigo", docente },
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_horarios_docentes_eliminar", parametros);

            if (!docente.IsNullOrEmpty() && institucion.ValidarPersonalizacion(Constantes.ZPERSONALIZACION_SUGERENCIAS_AZURE_SEARCH))
            {
                BackgroundJob.Enqueue(() => new ServicioSuggestions(institucion, usuario).ActualizarAccesoDocentesAdministrativosJob(institucion.aplent_id, docente, "D", null, null));
            }
        }

        public void EliminarTodosHorariosDocentes(int planAcademico, string docente)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaaca_consecutivoP", planAcademico },
                { "hordoc_per_codigo", docente },
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_horarios_docentes_todos_eliminar", parametros);

            if (!docente.IsNullOrEmpty() && institucion.ValidarPersonalizacion(Constantes.ZPERSONALIZACION_SUGERENCIAS_AZURE_SEARCH))
            {
                BackgroundJob.Enqueue(() => new ServicioSuggestions(institucion, usuario).ActualizarAccesoDocentesAdministrativosJob(institucion.aplent_id, docente, "D", null, null));
            }
        }

        #endregion

        #region Horario Cursos rotativos API

        public async Task<IPagedList<HorarioViewModel>> ObtenerHorariosCursosRotativosApi(int? ConsecutivoAnioLectivo, int? ConcecutivoCursoRotativo = null, int? ConsecutivoGrado = null, string CodigoAsignatura = null, DateTime? FechaInicio = null, DateTime? FechaFin = null, bool? incluirCursosArchivados = null, int? ConsecutivoAulaFisica = null, string CodigoAulaVirtual = null, int pagina = 1, int tamanioPagina = Constantes.PAGINACION_TAMANO_PAGINA)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaaca_per_consecutivo", ConsecutivoAnioLectivo },
                { "filtros_plan_academico", ConcecutivoCursoRotativo },
                { "filtros_aula_fisica", ConsecutivoAulaFisica },
                { "filtros_aula_virtual", CodigoAulaVirtual },
                { "filtros_fecha_inicio", FechaInicio },
                { "filtros_fecha_fin", FechaFin },
                { "filtros_asignatura", CodigoAsignatura },
                { "filtros_grado", ConsecutivoGrado },
                { "incluir_cursos_archivados", incluirCursosArchivados },
            };

            var result = await institucion.BaseDatos.ExecuteQueryAsync<HorarioViewModel>("pa_apis_aca_horarios_colegios_rotativos_retornar", parametros, pagina, tamanioPagina);
            return new StaticPagedList<HorarioViewModel>(result.Item1, pagina, tamanioPagina, result.Item2);
        }

        #endregion Horario Cursos rotativos API

        #endregion

        #region Horarios Estandar
        public IList<HorarioEstandarViewModel> ObtenerHorariosSemanales(int? consecutivoPlan, string codigoDocente = null, int? consecutivoAula = null, DateTime? fechaInicio = null, DateTime? fechaFin = null, string codigoAulaVirtual = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaaca_consecutivoP", consecutivoPlan },
                { "aul_consecutivoP", consecutivoAula },
                { "plaaca_per_codigo_docente", codigoDocente },
                { "fecha_inicio", fechaInicio },
                { "fecha_fin", fechaFin },
                { "aulvir_codigoP", codigoAulaVirtual }
            };

            return institucion.BaseDatos.ExecuteQuery<HorarioEstandarViewModel>("pa_aca_horarios_semanales_retornar", parametros);
        }

        public int IngresarHorarioSemanal(int? consecutivoPlan, int? consecutivoAula, char? dia, TimeSpan? fechaInicio, TimeSpan? fechaFinal, int? consecutivoRep, string aulaVirtual = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "horsem_plaaca_consecutivo", consecutivoPlan },
                { "horsem_aul_consecutivo", consecutivoAula },
                { "horsem_dia", dia },
                { "horsem_hora_inicio", fechaInicio },
                { "horsem_hora_fin", fechaFinal },
                { "horsem_rephor_consecutivo", consecutivoRep },
                { "horsem_aulvir_codigo", aulaVirtual },
            };

            return institucion.BaseDatos.ExecuteNonQuery("pa_aca_horarios_semanales_ingresar", parametros);
        }

        public void EliminarHorarioSemanal(int consecutivoHorario)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "horsem_consecutivoP", consecutivoHorario}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_horarios_semanales_eliminar", parametros);
        }

        public void ModificarHorarioSemanal(int? consecutivoHorario, int? consecutivoAula, TimeSpan? fechaInicio, TimeSpan? fechaFinal, string codigoAulaVirtual = null, string dias = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "horsem_consecutivoP", consecutivoHorario },
                { "horsem_aul_consecutivo", consecutivoAula },
                { "horsem_hora_inicio", fechaInicio },
                { "horsem_hora_fin", fechaFinal },
                { "horsem_aulvir_codigo", codigoAulaVirtual },
                { "horsem_dia", dias }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_horarios_semanales_modificar", parametros);
        }

        public HorarioEstandarViewModel ObtenerDetalleHorariosSemanales(int? consecutivoHorario)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "horsem_consecutivoP", consecutivoHorario }
            };

            return institucion.BaseDatos.ExecuteQuery<HorarioEstandarViewModel>("pa_aca_horarios_semanales_detalle_retornar", parametros).FirstOrDefault();
        }

        public IList<dynamic> ObtenerHorariosConflicto(int? codigoHorario, int? codigoAula, string codigoDocente, TimeSpan? horaInicio, TimeSpan? horaFin, int? repeticion, DateTime fechaInicio, DateTime fechaFin, string dias, int? planAcademico = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "hor_consecutivoP", codigoHorario },
                { "hor_aul_consecutivo", codigoAula },
                { "plaaca_per_codigo_docente", codigoDocente },
                { "hor_inicio", horaInicio },
                { "hor_fin", horaFin },
                { "hor_rephor_consecutivo", repeticion },
                { "fecha_inicio", fechaInicio },
                { "fecha_fin", fechaFin},
                { "dias", dias},
                { "plan_academico_omitir",  planAcademico}
            };

            var result = institucion.BaseDatos.ExecuteQuery<dynamic>("pa_aca_horarios_conflicto_por_repeticion_revisar", parametros);
            return result.Where(m => m.horario_Conflicto != null).ToList();
        }

        public string ObtenerHorariosConflictoColegio(int? codigoHorario, int? codigoAula, string codigoDocente, TimeSpan? horaInicio, TimeSpan? horaFin, int? repeticion, DateTime fechaInicio, DateTime fechaFin, string dias, int? grupo, int? planAcademico)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "hor_consecutivoP", codigoHorario },
                { "hor_aul_consecutivo", codigoAula },
                { "plaaca_per_codigo_docente", codigoDocente },
                { "hor_inicio", horaInicio },
                { "hor_fin", horaFin },
                { "hor_rephor_consecutivo", repeticion },
                { "fecha_inicio", fechaInicio },
                { "fecha_fin", fechaFin },
                { "dias", dias },
                { "plaaca_gru_consecutivo", grupo },
                { "plaaca_consecutivoP", planAcademico }
            };

            var result = institucion.BaseDatos.ExecuteQuery<dynamic>("pa_aca_horarios_conflicto_por_repeticion_colegios_revisar", parametros);
            result = result.Where(m => m.horario_Conflicto != null).ToList();
            if (result == null || result.Count() == 0) return string.Empty;

            return string.Join(",", result.GroupBy(m => m.asi_nombre).Select(m => m.First().asi_nombre));
        }

        public bool ValidarHorariosRangoFechas(DateTime fechaInicio, DateTime fechaFin, string codigoDocente)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "hor_fecha_inicio ", fechaInicio },
                { "hor_fecha_fin ", fechaFin },
                { "hordoc_per_codigo ", codigoDocente }
            };

            return Convert.ToBoolean(institucion.BaseDatos.ExecuteNonQuery("pa_aca_horarios_docentes_validar", parametros));
        }

        public bool ValidarHorarioRangoFecha(DateTime fechaInicio, DateTime fechaFin, int planAcademico, int? consecutivoHorario = null, int? consecutivoRepeticion = null ,bool? tieneRepeticion = false, DateTime? repetirHasta = null, string diasRepeticion = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "hor_fecha_inicio ", fechaInicio },
                { "hor_fecha_fin ", fechaFin },
                { "hor_consecutivoP", consecutivoHorario },
                { "plaaca_consecutivoP ", planAcademico },
                { "hor_rephor_consecutivo", consecutivoRepeticion },
                { "tieneRepeticion", tieneRepeticion },
                { "dias_repeticion", diasRepeticion },
                { "repetir_hasta", repetirHasta },
            };

            return Convert.ToBoolean(institucion.BaseDatos.ExecuteNonQuery("pa_aca_planes_academicos_validar_cruce_horario_curso", parametros));
        }

        public bool ValidarHorarioRangoFechaColombo(DateTime fechaInicio, DateTime fechaFin, int planAcademico, DateTime? repetirHasta = null, string diasRepeticion = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "hor_fecha_inicio ", fechaInicio },
                { "hor_fecha_fin ", fechaFin },
                { "plaaca_consecutivoP ", planAcademico },
                { "dias_repeticion", diasRepeticion },
                { "repetir_hasta", repetirHasta },
            };

            return Convert.ToBoolean(institucion.BaseDatos.ExecuteNonQuery("paz_colombo_aca_planes_academicos_validar_cruce_horario_curso", parametros));
        }

        #endregion

        #region Horarios ASignaturas

        public List<HorarioViewModel> ObtenerDocentesAsignaturaHorario(string asignatura)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_asignatura", asignatura }
            };

            return institucion.BaseDatos.ExecuteQuery<HorarioViewModel>("pa_aca_horarios_ultimos_docentes_retornar", parametros).ToList();
        }

        #endregion

        #region Api horarios cursos

        public async Task<IPagedList<HorarioViewModel>> ObtenerHorariosCursoApi(DateTime fechaInicio, DateTime fechaFin, int? consecutivoCurso, string personaCodigo = null, int? consecutivoAulaFisca = null, string codigoAulaVirtual = null, bool incluirCursosArchivados = false, int pagina = 1, int tamanioPagina = Constantes.PAGINACION_TAMANO_PAGINA)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_fecha_inicio", fechaInicio },
                { "filtros_fecha_fin", fechaFin },
                { "filtros_plan_academico", consecutivoCurso },
                { "filtros_docente", personaCodigo },
                { "filtros_aula_fisica", consecutivoAulaFisca },
                { "filtros_aula_virtual", codigoAulaVirtual },
                { "incluir_cursos_archivados", incluirCursosArchivados },
                { "hor_consecutivoP", null },
            };

            var result = await institucion.BaseDatos.ExecuteQueryAsync<HorarioViewModel>("pa_apis_aca_horarios_retornar", parametros, pagina, tamanioPagina);
            return new StaticPagedList<HorarioViewModel>(result.Item1, pagina, tamanioPagina, result.Item2);
        }

        public async Task<HorarioViewModel> ObtenerDetalleHorarioCursoApi(int id)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "hor_consecutivoP", id },
            };

            return (await institucion.BaseDatos.ExecuteQueryAsync<HorarioViewModel>("pa_apis_aca_horarios_retornar", parametros)).FirstOrDefault();
        }

        #endregion Api horarios cursos

        #region Unidades

        public IList<UnidadViewModel> ObtenerUnidades(string codigoModulo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "uni_mod_codigo", codigoModulo }
            };

            return institucion.BaseDatos.ExecuteQuery<UnidadViewModel>("pa_aca_unidades_aprendizaje_retornar", parametros);
        }

        public UnidadViewModel ObtenerDetalleUnidad(int codigoUnidad)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "uni_consecutivoP", codigoUnidad }
            };

            return institucion.BaseDatos.ExecuteQuery<UnidadViewModel>("pa_aca_unidades_aprendizaje_detalle_retornar", parametros).FirstOrDefault();
        }

        public void IngresarUnidad(string codigoModulo, string nombre, string abreviacion)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "uni_mod_codigo", codigoModulo },
                { "uni_nombre", nombre },
                { "uni_abreviacion", abreviacion}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_unidades_aprendizaje_ingresar", parametros);
        }

        public void ModificarUnidad(int codigo, string nombre, string abreviacion)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "uni_consecutivoP", codigo },
                { "uni_nombre", nombre },
                { "uni_abreviacion", abreviacion}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_unidades_aprendizaje_modificar", parametros);
        }

        public void EliminarUnidad(int codigo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "uni_consecutivoP", codigo}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_unidades_aprendizaje_eliminar", parametros);
        }

        public async Task<IList<ActividadViewModel>> ObtenerActividadesCompetenciasAPIAsync(string consecutivosMatriculaPlan)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "matpla_consecutivos", consecutivosMatriculaPlan }
            };

            return await institucion.BaseDatos.ExecuteQueryAsync<ActividadViewModel>("pa_aca_actividades_aprendizaje_retornar", parametros);
        }

        #endregion

        #region Actividades

        public IList<ActividadViewModel> ObtenerActividades(int? codigoUnidad, string modulo = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "act_uni_consecutivo", codigoUnidad },
                { "mod_codigo", modulo }
            };

            return institucion.BaseDatos.ExecuteQuery<ActividadViewModel>("pa_aca_actividades_aprendizaje_retornar", parametros);
        }

        public void ReordenarActividades(int codigoUnidad, int ordenAnterior, int ordenNuevo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "act_uni_consecutivo", codigoUnidad },
                { "act_orden_anterior", ordenAnterior },
                { "act_orden_nuevo", ordenNuevo }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_actividades_aprendizaje_reordenar", parametros);
        }

        public ActividadViewModel ObtenerDetalleActividad(int codigoActividad)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "act_consecutivoP", codigoActividad }
            };

            return institucion.BaseDatos.ExecuteQuery<ActividadViewModel>("pa_aca_actividades_aprendizaje_detalle_retornar", parametros).FirstOrDefault();
        }

        public void IngresarActividad(int codigoUnidad, string nombre, string abreviacion)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "act_uni_consecutivo", codigoUnidad },
                { "act_nombre", nombre },
                { "act_abreviacion", abreviacion}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_actividades_aprendizaje_ingresar", parametros);
        }

        public void ModificarActividad(int codigo, string nombre, string abreviacion)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "act_consecutivoP", codigo },
                { "act_nombre", nombre },
                { "act_abreviacion", abreviacion}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_actividades_aprendizaje_modificar", parametros);
        }

        public void EliminarActividad(int codigo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "act_consecutivoP", codigo}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_actividades_aprendizaje_eliminar", parametros);
        }

        #endregion

        #region Evidencias

        public IList<EvidenciaViewModel> ObtenerEvidencias(int? codigoActividad)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "evi_act_consecutivo", codigoActividad }
            };

            return institucion.BaseDatos.ExecuteQuery<EvidenciaViewModel>("pa_aca_evidencias_aprendizaje_retornar", parametros);
        }

        public EvidenciaViewModel ObtenerDetalleEvidencia(int codigoEvidencia)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "evi_consecutivoP", codigoEvidencia }
            };

            return institucion.BaseDatos.ExecuteQuery<EvidenciaViewModel>("pa_aca_evidencias_aprendizaje_detalle_retornar", parametros).FirstOrDefault();
        }

        public void IngresarEvidencia(int codigoActividad, string nombre, string abreviacion, char tipo, int? puntajeEvaluacion, int? intentos)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "evi_act_consecutivo", codigoActividad },
                { "evi_nombre", nombre },
                { "evi_abreviacion", abreviacion },
                { "evi_tipo", tipo },
                { "evi_puntaje_evaluacion", puntajeEvaluacion },
                { "evi_intentos", intentos}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_evidencias_aprendizaje_ingresar", parametros);
        }

        public void ModificarEvidencia(int codigo, string nombre, string abreviacion, char tipo, int? puntajeEvaluacion, int? intentos)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "evi_consecutivoP", codigo },
                { "evi_nombre", nombre },
                { "evi_abreviacion", abreviacion },
                { "evi_tipo", tipo },
                { "evi_puntaje_evaluacion", puntajeEvaluacion },
                { "evi_intentos", intentos}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_evidencias_aprendizaje_modificar", parametros);
        }

        public void EliminarEvidencia(int codigo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "evi_consecutivoP", codigo}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_evidencias_aprendizaje_eliminar", parametros);
        }

        public bool ValidarNotasEvidencia(int codigo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "evi_consecutivoP", codigo}
            };

            return Convert.ToBoolean(institucion.BaseDatos.ExecuteNonQuery("pa_aca_evidencias_aprendizaje_validar", parametros));
        }

        #endregion

        #region Indicadores de logros
        public IList<IndicadorLogroViewModel> ObtenerIndicadoresLogrosParaCopiar(int? grado, int? periodo, string asignatura, int? gradoPeriodo, char? tipo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "ind_gra_consecutivo", grado },
                { "ind_per_consecutivo", periodo },
                { "ind_asi_codigo", asignatura },
                { "ind_graper_consecutivo", gradoPeriodo },
                { "ind_tipo", tipo }
            };

            return institucion.BaseDatos.ExecuteQuery<IndicadorLogroViewModel>("pa_aca_indicadores_logros_para_copiar_retornar", parametros);
        }

        public IList<IndicadorLogroViewModel> ObtenerIndicadoresLogros(string texto, bool? estado, int? grado, int? periodo, string asignatura, int? tipoIndicador, int? gradoPeriodo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "ind_gra_consecutivo", grado },
                { "ind_per_consecutivo", periodo },
                { "ind_asi_codigo", asignatura },
                { "ind_tipind_consecutivo", tipoIndicador },
                { "ind_graper_consecutivo", gradoPeriodo },
                { "filtros_estado", estado },
                { "filtros_texto", texto }
            };

            return institucion.BaseDatos.ExecuteQuery<IndicadorLogroViewModel>("pa_aca_indicadores_logros_retornar", parametros);
        }

        public IndicadorLogroViewModel ObtenerDetalleIndicadorLogro(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "ind_consecutivoP", consecutivo },
            };

            return institucion.BaseDatos.ExecuteQuery<IndicadorLogroViewModel>("pa_aca_indicadores_logros_detalle_retornar", parametros).FirstOrDefault();
        }

        public async Task<IndicadorLogroViewModel> ObtenerDetalleIndicadorLogroAsync(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "ind_consecutivoP", consecutivo },
            };

            return (await institucion.BaseDatos.ExecuteQueryAsync<IndicadorLogroViewModel>("pa_aca_indicadores_logros_detalle_retornar", parametros)).FirstOrDefault();
        }

        public void IngresarIndicadorLogro(string codigo, string descripcion, int grado, int periodo, string asignatura, int tipoIndicador, bool estado, int? gradoPeriodo, bool aplicaAutomatizacion)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "ind_codigo", codigo },
                { "ind_descripcion", descripcion },
                { "ind_gra_consecutivo", grado },
                { "ind_per_consecutivo", periodo },
                { "ind_asi_codigo", asignatura },
                { "ind_tipind_consecutivo", tipoIndicador },
                { "ind_graper_consecutivo", gradoPeriodo },
                { "ind_estado", estado },
                { "ind_aplica_automatizacion", aplicaAutomatizacion}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_indicadores_logros_ingresar", parametros);
        }

        public void ModificarIndicadorLogro(int consecutivo, string codigo, string descripcion, int tipoIndicador, bool estado, bool aplicaAutomatizacion)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "ind_consecutivoP", consecutivo },
                { "ind_codigo", codigo },
                { "ind_descripcion", descripcion },
                { "ind_tipind_consecutivo", tipoIndicador },
                { "ind_estado", estado },
                { "ind_aplica_automatizacion", aplicaAutomatizacion}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_indicadores_logros_modificar", parametros);
        }

        public void EliminarIndicadorLogro(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "ind_consecutivoP", consecutivo },
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_indicadores_logros_eliminar", parametros);
        }

        public int ObtenerValidacionIndicadoresLogros(int? idgrado_desde, int? idperiodo_desde, int? idgrado_hacia, int? idperiodo_hacia)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "ind_gra_consecutivo_desde", idgrado_desde },
                { "ind_per_consecutivo_desde", idperiodo_desde },
                { "ind_gra_consecutivo_hacia", idgrado_hacia },
                { "ind_per_consecutivo_hacia", idperiodo_hacia }
            };

            return institucion.BaseDatos.ExecuteNonQuery("pa_aca_indicadores_logros_validar", parametros);

        }

        public void CopiaIndicadoresLogros(int? idgrado_desde, int? idperiodo_desde, int? idgradoperiodo_desde, string idasignatura_desde, int? idgrado_hacia, int? idperiodo_hacia, int? idgradoperiodo_hacia, string idasignatura_hacia, char? tipo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "ind_gra_consecutivo_desde", idgrado_desde },
                { "ind_per_consecutivo_desde", idperiodo_desde },
                { "ind_asi_codigo_desde", idasignatura_desde },
                { "ind_graper_consecutivo_desde", idgradoperiodo_desde },
                { "ind_gra_consecutivo_hacia", idgrado_hacia },
                { "ind_per_consecutivo_hacia", idperiodo_hacia },
                { "ind_asi_codigo_hacia", idasignatura_hacia },
                { "ind_graper_consecutivo_hacia",idgradoperiodo_hacia },
                { "ind_tipo",tipo },

            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_indicadores_logros_copiar", parametros);
        }

        public void CopiaMasivaIndicadoresLogros(int? idgrado_desde, int? idperiodo_desde, string idasignatura_desde, int? idgrado_hacia, int? idperiodo_hacia, string idasignatura_hacia)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "ind_gra_consecutivo_desde", idgrado_desde },
                { "ind_per_consecutivo_desde", idperiodo_desde },
                { "ind_asi_codigo_desde", idasignatura_desde },
                { "ind_gra_consecutivo_hacia", idgrado_hacia },
                { "ind_per_consecutivo_hacia", idperiodo_hacia },
                { "ind_asi_codigo_hacia", idasignatura_hacia }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_indicadores_logros_masivo_copiar", parametros);
        }

        public bool ValidarIndicadorLogroHistorico(int indicador, bool incluirAbiertos = false)
        {
            //true: hay información registrada con ese indicador
            var parametros = new Dictionary<string, object>(){
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "ind_consecutivo", indicador},
                { "incluir_periodos_abiertos", incluirAbiertos}
            };

            return Convert.ToBoolean(institucion.BaseDatos.ExecuteQuery<dynamic>("pa_aca_indicadores_logros_validar_historico", parametros).FirstOrDefault().enhistorico);
        }

        public void EliminarIndicadorLogroEstudiantesComfanorte(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "ind_consecutivoP", consecutivo },
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_matriculas_planes_indicadores_logros_no_archivados_eliminar", parametros);
        }

        public void EliminarIndicadorLogroAsignaturasComfanorte(string asignatura)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "asi_codigoP", asignatura },
            };

            institucion.BaseDatos.ExecuteNonQuery("paz_comfanorte_aca_indicadores_logros_asignatura_eliminar", parametros);
        }

        #region Indicadores Apis
        public void IngresarIndicadorLogroApi(string codigo, string descripcion, int grado, int periodo, string asignatura, int tipoIndicador, bool estado, int? gradoPeriodo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "ind_codigo", codigo },
                { "ind_descripcion", descripcion },
                { "ind_gra_consecutivo", grado },
                { "ind_per_consecutivo", periodo },
                { "ind_asi_codigo", asignatura },
                { "ind_tipind_consecutivo", tipoIndicador },
                { "ind_graper_consecutivo", gradoPeriodo },
                { "ind_estado", estado }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_apis_aca_indicadores_logros_ingresar", parametros);
        }

        public async Task<IList<IndicadorLogroViewModel>> ObtenerIndicadoresLogrosApiAsync(bool? estado, int grado, int periodo, string asignatura)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "ind_gra_consecutivo", grado },
                { "ind_per_consecutivo", periodo },
                { "ind_asi_codigo", asignatura },
                { "filtros_estado", estado }
             };

            return await institucion.BaseDatos.ExecuteQueryAsync<IndicadorLogroViewModel>("pa_apis_aca_indicadores_logros_retornar", parametros);
        }

        public async Task<IPagedList<IndicadorLogroViewModel>> ObtenerIndicadoresLogrosApiAsync(bool? estado, int grado, int periodo, string asignatura, int tamanioPagina, int pageIndex)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "ind_gra_consecutivo", grado },
                { "ind_per_consecutivo", periodo },
                { "ind_asi_codigo", asignatura },
                { "filtros_estado", estado },
            };

            var result = await institucion.BaseDatos.ExecuteQueryAsync<IndicadorLogroViewModel>("pa_apis_aca_indicadores_logros_retornar", parametros, pageIndex, tamanioPagina);

            return new StaticPagedList<IndicadorLogroViewModel>(result.Item1, pageIndex, tamanioPagina, result.Item2);
        }

        #endregion

        #endregion

        #region Parametros Indicadores

        public IList<ParametroIndicadorLogroViewModel> ObtenerIndicadoresLogrosParametro(int parametroEvaluacion, string asignatura, int periodo, int grado, int? graper = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "ind_gra_consecutivo", grado },
                { "ind_per_consecutivo", periodo },
                { "ind_asi_codigo", asignatura },
                { "ind_tipind_consecutivo", null },
                { "ind_graper_consecutivo", graper },
                { "filtros_estado", null },
                { "filtros_texto", null },
                { "ind_parevagra_consecutivo", parametroEvaluacion }
            };

            return institucion.BaseDatos.ExecuteQuery<ParametroIndicadorLogroViewModel>("pa_aca_indicadores_logros_retornar", parametros);
        }

        public void ModificarIndicadorLogroParametro(int consecutivo, int? parametroEvaluacion, decimal? notaInicio, decimal? notaFinal)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "ind_consecutivoP", consecutivo },
                { "ind_parevagra_consecutivo", parametroEvaluacion },
                { "ind_rango_inicio", notaInicio },
                { "ind_rango_fin", notaFinal }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_indicadores_logros_informacion_adicional_modificar", parametros);
        }

        #endregion Parametros Indicadores 

        #region Tipos Indicadores de logros

        public IList<TipoIndicadorLogroViewModel> ObtenerTiposIndicadores(string texto, bool? estado)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_estado", estado },
                { "filtros_texto", texto }
            };

            return institucion.BaseDatos.ExecuteQuery<TipoIndicadorLogroViewModel>("pa_aca_tipos_indicadores_logros_retornar", parametros);
        }

        public async Task<IPagedList<TipoIndicadorLogroViewModel>> ObtenerTiposIndicadoresApiAsync(bool? estado, int pagina, int tamanioPagina)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_estado", estado }
            };

            var result = await institucion.BaseDatos.ExecuteQueryAsync<TipoIndicadorLogroViewModel>("pa_apis_aca_tipos_indicadores_logros_retornar", parametros, pagina, tamanioPagina);
            return new StaticPagedList<TipoIndicadorLogroViewModel>(result.Item1, pagina, tamanioPagina, result.Item2);
        }

        public TipoIndicadorLogroViewModel ObtenerDetalleTipoIndicador(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "tipind_consecutivoP", consecutivo }
            };

            return institucion.BaseDatos.ExecuteQuery<TipoIndicadorLogroViewModel>("pa_aca_tipos_indicadores_logros_detalle_retornar", parametros).FirstOrDefault();
        }

        public void IngresarTipoIndicador(string nombre, string abreviacion, bool alcanzado, bool estado)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "tipind_nombre", nombre },
                { "tipind_abreviacion", abreviacion },
                { "tipind_alcanzado", alcanzado },
                { "tipind_estado", estado}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_tipos_indicadores_logros_ingresar", parametros);
        }

        public void ModificarTipoIndicador(int consecutivo, string nombre, string abreviacion, bool alcanzado, bool estado)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "tipind_consecutivoP", consecutivo },
                { "tipind_nombre", nombre },
                { "tipind_abreviacion", abreviacion },
                { "tipind_alcanzado", alcanzado },
                { "tipind_estado", estado}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_tipos_indicadores_logros_modificar", parametros);
        }

        public void EliminarTipoIndicador(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "tipind_consecutivoP", consecutivo}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_tipos_indicadores_logros_eliminar", parametros);
        }

        #endregion

        #region Pensum - Programa

        public IList<PensumProgramaViewModel> ObtenerPensumPrograma(string programa, bool? estado, char? estadoPensum = null, string usuario = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_programa", programa },
                { "pro_estado", estado },
                { "per_codigo", usuario },
                { "propen_estado", estadoPensum }
            };

            return institucion.BaseDatos.ExecuteQuery<PensumProgramaViewModel>("pa_aca_programas_pensum_retornar", parametros);
        }

        public IList<PensumProgramaViewModel> ObtenerPensumProgramasMasivos(string programas, bool? estado = null, char? estadoPensum = null, string usuario = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "programas", programas },
                { "pro_estado", estado },
                { "propen_estado", estadoPensum }
            };

            return institucion.BaseDatos.ExecuteQuery<PensumProgramaViewModel>("pa_aca_programas_pensum_masivo_retornar", parametros);
        }

        public PensumProgramaViewModel ObtenerDetallePensumPrograma(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "propen_consecutivoP", consecutivo },
            };

            return institucion.BaseDatos.ExecuteQuery<PensumProgramaViewModel>("pa_aca_programas_pensum_detalle_retornar", parametros).FirstOrDefault();
        }

        public int IngresarPensumPrograma(string programa, string nombre, char? estado)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "propen_pro_codigo", programa },
                { "propen_nombre", nombre },
                { "propen_estado", estado }
            };

            var id = institucion.BaseDatos.ExecuteQuery<PensumProgramaViewModel>("pa_aca_programas_pensum_ingresar", parametros).FirstOrDefault();
            return id.propen_consecutivoP;

        }

        public void ModificarPensumPrograma(int consecutivo, string nombre)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "propen_consecutivoP", consecutivo },
                { "propen_nombre", nombre }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_programas_pensum_nombre_modificar", parametros);
        }

        public void ModificarEstadoPensumPrograma(int consecutivo, char? estado)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "propen_consecutivoP", consecutivo },
                { "propen_estado", estado }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_programas_pensum_modificar", parametros);
        }

        public void EliminarPensumPrograma(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "propen_consecutivoP", consecutivo }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_programas_pensum_eliminar", parametros);
        }

        #endregion

        #region Vigencia - Asignatura

        public IList<VigenciaAsignaturaViewModel> ObtenerVigenciaAsignatura(string asignatura, bool? estado, char? estadoVigencia = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_asignaturas", asignatura },
                { "asi_estado", estado },
                { "asivig_estado", estadoVigencia },
            };

            return institucion.BaseDatos.ExecuteQuery<VigenciaAsignaturaViewModel>("pa_aca_asignaturas_vigencia_retornar", parametros);
        }

        public VigenciaAsignaturaViewModel ObtenerDetalleVigenciaAsignatura(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "asivig_consecutivoP", consecutivo },
            };

            return institucion.BaseDatos.ExecuteQuery<VigenciaAsignaturaViewModel>("pa_aca_asignaturas_vigencia_detalle_retornar", parametros).FirstOrDefault();
        }

        public int IngresarVigenciaAsignatura(string asignatura, string nombre, char? estado)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "asivig_asi_codigo", asignatura },
                { "asivig_nombre", nombre },
                { "asivig_estado", estado }
            };

            var id = institucion.BaseDatos.ExecuteQuery<VigenciaAsignaturaViewModel>("pa_aca_asignaturas_vigencia_ingresar", parametros).FirstOrDefault();
            return id.asivig_consecutivoP;

        }

        public void ModificarVigenciaAsignatura(int consecutivo, string nombre)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "asivig_consecutivoP", consecutivo },
                { "asivig_nombre", nombre }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_asignaturas_vigencia_nombre_modificar", parametros);
        }

        public void ModificarEstadoVigencia(int consecutivo, char? estado)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "asivig_consecutivoP", consecutivo },
                { "asivig_estado", estado }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_asignaturas_vigencia_estado_modificar", parametros);
        }

        public void EliminarVigenciaAsignatura(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "asivig_consecutivoP", consecutivo }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_asignaturas_vigencia_eliminar", parametros);
        }

        #endregion

        #region Educación Modular

        public IList<AsignaturaViewModel> ObtenerAsignaturasParaAsignarModulo(int? pensumPrograma)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "modpro_propen_consecutivo", pensumPrograma }
            };

            return institucion.BaseDatos.ExecuteQuery<AsignaturaViewModel>("pa_aca_asignaturas_para_asignar_modular_retornar", parametros);
        }

        public IList<ModuloPensumViewModel> ObtenerModulosParaAsignarPensum(int? pensumPrograma)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "modpro_propen_consecutivo", pensumPrograma }
            };

            return institucion.BaseDatos.ExecuteQuery<ModuloPensumViewModel>("pa_aca_modular_para_asignar_modular_retornar", parametros);
        }

        public IList<ModuloPensumViewModel> ObtenerModulosPensum(int? pensumPrograma)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "modpro_propen_consecutivo", pensumPrograma }
            };

            return institucion.BaseDatos.ExecuteQuery<ModuloPensumViewModel>("pa_aca_modular_programas_retornar", parametros);
        }

        public ModuloPensumViewModel ObtenerDetalleModuloPensum(int? modulo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "modpro_consecutivoP", modulo }
            };

            return institucion.BaseDatos.ExecuteQuery<ModuloPensumViewModel>("pa_aca_modular_programas_detalle_retornar", parametros).FirstOrDefault();
        }

        public void IngresarModuloPensumPrograma(int pensumPrograma, string modulo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "modpro_mod_codigo", modulo },
                { "modpro_propen_consecutivo", pensumPrograma },
                { "aud_usuario_sesion", per_codigoP },
                { "aud_ip",ClientIpAddress },
                { "aud_cat_codigo", CategoriaAuditoria },
                { "aud_usuario_q10", codigo_ejecutivo_institucion },
                { "aud_interno", false }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_modular_programas_ingresar", parametros);
        }

        public void EliminarModuloPensum(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "modpro_consecutivoP", consecutivo }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_modular_programas_eliminar", parametros);
        }

        #endregion

        #region Agrupaciones

        public IList<AgrupacionViewModel> ObtenerAgrupaciones(string texto, string programa, int? pensum)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_programa", programa },
                { "agr_propen_consecutivo", pensum },
                { "filtros_texto", texto }
            };

            return institucion.BaseDatos.ExecuteQuery<AgrupacionViewModel>("pa_aca_agrupaciones_retornar", parametros);
        }

        public AgrupacionViewModel ObtenerDetalleAgrupacion(string codigo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "agr_codigoP", codigo },
            };

            return institucion.BaseDatos.ExecuteQuery<AgrupacionViewModel>("pa_aca_agrupaciones_detalle_retornar", parametros).FirstOrDefault();
        }

        public void IngresarAgrupacion(string codigo, string nombre, string programa, int pensum)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "agr_codigoP", codigo },
                { "agr_nombre", nombre },
                { "agr_pro_codigo", programa },
                { "agr_propen_consecutivo", pensum }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_agrupaciones_ingresar", parametros);
        }

        public void ModificarAgrupacion(string codigo, string nombre)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "agr_codigoP", codigo },
                { "agr_nombre", nombre}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_agrupaciones_modificar", parametros);
        }

        public void EliminarAgrupacion(string codigo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "agr_codigoP", codigo }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_agrupaciones_eliminar", parametros);
        }

        #endregion

        #region Asignaturas - Agrupacion

        public IList<AsignaturaViewModel> ObtenerAsignaturasParaAsignarAgrupacion(string agrupacion)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "agr_codigo", agrupacion }
            };

            return institucion.BaseDatos.ExecuteQuery<AsignaturaViewModel>("pa_aca_asignaturas_para_asignar_agrupaciones_retornar", parametros);
        }

        public IList<AsignaturaAgrupacionViewModel> ObtenerAsignaturasAgrupacion(string texto, string agrupacion)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_agrupador", agrupacion },
                { "filtros_texto", texto }
            };

            return institucion.BaseDatos.ExecuteQuery<AsignaturaAgrupacionViewModel>("pa_aca_agrupaciones_asignaturas_retornar", parametros);
        }

        public AsignaturaAgrupacionViewModel ObtenerDetalleAsignaturaAgrupacion(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "agrasi_consecutivoP", consecutivo },
            };

            return institucion.BaseDatos.ExecuteQuery<AsignaturaAgrupacionViewModel>("pa_aca_agrupaciones_asignaturas_detalle_retornar", parametros).FirstOrDefault();
        }

        public void IngresarAsignaturaAgrupacion(string agrupacion, string asignatura, decimal? porcentaje)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "agrasi_agr_codigo", agrupacion },
                { "agrasi_asi_codigo", asignatura },
                { "agrasi_porcentaje", porcentaje }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_agrupaciones_asignaturas_ingresar", parametros);
        }

        public void ModificarAsignaturaAgrupacion(int consecutivo, decimal? porcentaje)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "agrasi_consecutivoP", consecutivo },
                { "agrasi_porcentaje", porcentaje}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_agrupaciones_asignaturas_modificar", parametros);
        }

        public void EliminarAsignaturaAgrupacion(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "agrasi_consecutivoP", consecutivo }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_agrupaciones_asignaturas_eliminar", parametros);
        }

        #endregion

        #region Capacidades Terminales

        public IList<CapacidadTerminalViewModel> ObtenerCapacidadesTerminales(string asignatura)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_asignaturas", asignatura }
            };

            return institucion.BaseDatos.ExecuteQuery<CapacidadTerminalViewModel>("pa_aca_capacidades_terminales_retornar", parametros);
        }

        public async Task<IList<CapacidadTerminalViewModel>> ObtenerCapacidadesTerminalesAsync(string consecutivosEstudiantes)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "matpla_consecutivos", consecutivosEstudiantes }
            };

            return await institucion.BaseDatos.ExecuteQueryAsync<CapacidadTerminalViewModel>("pa_apis_aca_capacidades_terminales_retornar", parametros);
        }

        public IList<CapacidadTerminalViewModel> ObtenerCapacidadesTerminalesHistoricas(string asignatura, int periodo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_asignaturas", asignatura },
                { "filtros_periodos", periodo }
            };

            return institucion.BaseDatos.ExecuteQuery<CapacidadTerminalViewModel>("pa_aca_hist_capacidades_terminales_retornar", parametros);
        }
        public CapacidadTerminalViewModel ObtenerDetalleCapacidadTerminal(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "capter_consecutivoP", consecutivo }
            };

            return institucion.BaseDatos.ExecuteQuery<CapacidadTerminalViewModel>("pa_aca_capacidades_terminales_detalle_retornar", parametros).FirstOrDefault();
        }

        public void IngresarCapacidadTeminal(string asignatura, string nombre, string abreviacion, int ordenamiento, decimal porcentaje)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "capter_nombre", nombre },
                { "capter_abreviacion", abreviacion },
                { "capter_ordenamiento", ordenamiento },
                { "capter_asi_codigo", asignatura },
                { "capter_porcentaje", porcentaje}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_capacidades_terminales_ingresar", parametros);
        }

        public void ModificarCapacidadTerminal(int consecutivo, string nombre, string abreviacion, int ordenamiento, decimal porcentaje)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "capter_consecutivoP", consecutivo },
                { "capter_nombre", nombre },
                { "capter_ordenamiento", ordenamiento },
                { "capter_abreviacion", abreviacion },
                { "capter_porcentaje", porcentaje }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_capacidades_terminales_modificar", parametros);
        }

        public void EliminarCapacidadTerminal(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "capter_consecutivoP", consecutivo }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_capacidades_terminales_eliminar", parametros);
        }

        #endregion

        #region Criterios de Evaluación

        public IList<CriterioEvaluacionViewModel> ObtenerCriteriosEvaluacion(int capacidadTerminal)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "crieva_capter_consecutivo", capacidadTerminal }
            };

            return institucion.BaseDatos.ExecuteQuery<CriterioEvaluacionViewModel>("pa_aca_criterios_evaluaciones_retornar", parametros);
        }
        public async Task<IList<CriterioEvaluacionViewModel>> ObtenerCriteriosEvaluacionAsync(string consecutivosEstudiantes)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "matpla_consecutivos", consecutivosEstudiantes }
            };

            return await institucion.BaseDatos.ExecuteQueryAsync<CriterioEvaluacionViewModel>("pa_apis_aca_criterios_evaluaciones_retornar", parametros);
        }

        public IList<CriterioEvaluacionViewModel> ObtenerCriteriosEvaluacionHistoricos(int capacidadTerminal, int periodo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "hcrieva_hcapter_consecutivo", capacidadTerminal },
                { "per_consecutivoP", periodo }
            };

            return institucion.BaseDatos.ExecuteQuery<CriterioEvaluacionViewModel>("pa_aca_hist_criterios_evaluaciones_retornar", parametros);
        }

        public CriterioEvaluacionViewModel ObtenerDetalleCriterioEvaluacion(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "crieva_consecutivoP", consecutivo },
            };

            return institucion.BaseDatos.ExecuteQuery<CriterioEvaluacionViewModel>("pa_aca_criterios_evaluaciones_detalle_retornar", parametros).FirstOrDefault();
        }

        public void IngresarCriterioEvaluacion(int capacidadTerminal, string nombre, string abreviacion, decimal? porcentaje = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "crieva_nombre", nombre },
                { "crieva_abreviacion", abreviacion },
                { "crieva_capter_consecutivo", capacidadTerminal },
                { "crieva_porcentaje", porcentaje }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_criterios_evaluaciones_ingresar", parametros);
        }

        public void ModificarCriterioEvaluacion(int consecutivo, string nombre, string abreviacion, decimal? porcentaje = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "crieva_consecutivoP", consecutivo },
                { "crieva_nombre", nombre },
                { "crieva_abreviacion", abreviacion },
                { "crieva_porcentaje", porcentaje }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_criterios_evaluaciones_modificar", parametros);
        }

        public void EliminarCriterioEvaluacion(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "crieva_consecutivoP", consecutivo }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_criterios_evaluaciones_eliminar", parametros);
        }

        #endregion

        #region Escala Nacionales

        public IList<EscalaNacionalViewModel> ObtenerEscalaNacional(int? PeriodoConsecutivo, string texto, bool aplicaTransicionEvaluacion = false /*Personalización 317*/)
        {
            var parametros = new Dictionary<string, object>(){
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "escnac_per_consecutivo", PeriodoConsecutivo },
                { "filtros_texto", texto },
                { "aplica_transicion", aplicaTransicionEvaluacion}
            };
            return institucion.BaseDatos.ExecuteQuery<EscalaNacionalViewModel>("pa_aca_escalas_nacionales_retornar", parametros);
        }

        public EscalaNacionalViewModel ObtenerDetalleEscalaNacional(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "escnac_consecutivoP", consecutivo}
            };

            return institucion.BaseDatos.ExecuteQuery<EscalaNacionalViewModel>("pa_aca_escalas_nacionales_detalle_retornar", parametros).FirstOrDefault();
        }

        public int IngresarEscalaNacional(int periodo, string nombre, string abreviacion, bool aprueba, decimal? escalaInicial = null, decimal? escalaFinal = null, string criterioAcademico = null,
            string nombreComfama = null, string abreviacionComfama = null, decimal? porcInicial = null, decimal? porcFinal = null, decimal? notaFinal = null, string codigoNiveles = null, int commandTimeout = 180)
        {
            var parametros = new Dictionary<string, object>(){
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "escnac_per_consecutivo", periodo },
                { "escnac_nombre",nombre },
                { "escnac_abreviacion", abreviacion },
                { "escnac_aprueba",aprueba },
                { "escnac_escala_inicial",escalaInicial },
                { "escnac_escala_final",escalaFinal },
                { "escnac_criterio_academico", criterioAcademico },
                { "escnac_nombre_comfama", nombreComfama },
                { "escnac_abreviacion_comfama", abreviacionComfama },
                { "escnac_porcentaje_inicial", porcInicial },
                { "escnac_porcentaje_final", porcFinal },
                { "escnac_nota_final", notaFinal },
                { "escnac_niv_codigos", codigoNiveles }
            };

            return institucion.BaseDatos.ExecuteNonQuery("pa_aca_escalas_nacionales_ingresar", parametros, commandTimeout: commandTimeout);
        }

        public void ModificarEscalaNacional(int consecutivo, int periodo, string nombre, string abreviacion, bool aprueba, decimal? escalaInicial, decimal? escalaFinal, string criterioAcademico,
            string nombreComfama = null, string abreviacionComfama = null, decimal? porcInicial = null, decimal? porcFinal = null, decimal? notaFinal = null, string codigoNiveles = null, int commandTimeout = 180)
        {
            var parametros = new Dictionary<string, object>(){
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "escnac_consecutivoP", consecutivo },
                { "escnac_per_consecutivo", periodo },
                { "escnac_nombre",nombre },
                { "escnac_abreviacion", abreviacion },
                { "escnac_aprueba",aprueba },
                { "escnac_escala_inicial",escalaInicial },
                { "escnac_escala_final",escalaFinal },
                { "escnac_criterio_academico", criterioAcademico },
                { "escnac_nombre_comfama", nombreComfama },
                { "escnac_abreviacion_comfama", abreviacionComfama },
                { "escnac_porcentaje_inicial", porcInicial },
                { "escnac_porcentaje_final", porcFinal },
                { "escnac_nota_final", notaFinal },
                { "escnac_niv_codigos", codigoNiveles }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_escalas_nacionales_modificar", parametros, commandTimeout: commandTimeout);
        }

        public void EliminarEscalaNacional(int consecutivo, int commandTimeout = 180)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "escnac_consecutivoP", consecutivo}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_escalas_nacionales_eliminar", parametros, commandTimeout: commandTimeout);
        }

        public void ModificarArchivoEscalaNacional(int escalaNacional, int? archivoAdjunto = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP  },
                { "escnac_consecutivoP", escalaNacional },
                { "escnac_arc_consecutivo", archivoAdjunto }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_escalas_nacionales_archivo_modificar", parametros);
        }

        #region API Escalas Nacionales

        public async Task<int> IngresarEscalaNacionalApiAsync(string nombre, string abreviatura, int? consecutivoAnioLectivo, bool? aprueba, decimal? escalaInicial, decimal? escalaFinal)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "escnac_nombre",nombre },
                { "escnac_abreviacion", abreviatura },
                { "escnac_per_consecutivo", consecutivoAnioLectivo },
                { "escnac_aprueba",aprueba },
                { "escnac_escala_inicial",escalaInicial },
                { "escnac_escala_final",escalaFinal }
            };

            return await institucion.BaseDatos.ExecuteNonQueryAsync("pa_apis_aca_escalas_nacionales_ingresar", parametros);
        }

        #endregion Fin API Escalas Nacionales

        #endregion Escala Nacionales

        #region Categorias Cancelación Practicas Laborales

        public IList<CategoriaCancelacionViewModel> ObtenerCategoriasCancelacion(string texto, bool? estado)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_texto", texto },
                { "filtro_estado", estado }
            };

            return institucion.BaseDatos.ExecuteQuery<CategoriaCancelacionViewModel>("pa_aca_tipos_razones_retornar", parametros);
        }

        public CategoriaCancelacionViewModel ObtenerDetalleCategoriaCancelacion(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "tipraz_consecutivoP", consecutivo }
            };

            return institucion.BaseDatos.ExecuteQuery<CategoriaCancelacionViewModel>("pa_aca_tipos_razones_detalle_retornar", parametros).FirstOrDefault();
        }

        public void IngresarCategoriaCancelacion(string nombre, bool estado)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "tipraz_nombre", nombre },
                { "tipraz_estado", estado }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_tipos_razones_ingresar", parametros);
        }

        public void ModificarCategoriaCancelacion(int consecutivo, string nombre, bool estado)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "tipraz_consecutivoP", consecutivo },
                { "tipraz_nombre", nombre },
                { "tipraz_estado", estado }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_tipos_razones_modificar", parametros);
        }

        public void EliminarCategoriaCancelacion(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "tipraz_consecutivoP", consecutivo}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_tipos_razones_eliminar", parametros);
        }

        #endregion

        #region Tipos de Contratos
        public IList<TipoContratoViewModel> ObtenerTiposContratos(bool? estado, string texto)
        {
            var parametros = new Dictionary<string, object>(){
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtro_texto", texto },
                { "filtros_estado", estado }
            };
            return institucion.BaseDatos.ExecuteQuery<TipoContratoViewModel>("pa_ofe_tipos_contratos_retornar", parametros);
        }

        public TipoContratoViewModel ObtenerDetalleTipoContrato(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "tipcon_consecutivoP", consecutivo }
            };

            return institucion.BaseDatos.ExecuteQuery<TipoContratoViewModel>("pa_ofe_tipos_contratos_detalle_retornar", parametros).FirstOrDefault();
        }

        public int IngresarTipoContrato(string nombre, bool estado, bool practica)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "tipcon_nombre", nombre },
                { "tipcon_estado", estado },
                { "tipcon_practica", practica }
            };

            return institucion.BaseDatos.ExecuteNonQuery("pa_ofe_tipos_contratos_ingresar", parametros);
        }

        public void ModificarTipoContrato(int consecutivo, string nombre, bool estado, bool practica)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "tipcon_consecutivoP", consecutivo },
                { "tipcon_nombre", nombre },
                { "tipcon_estado", estado },
                { "tipcon_practica", practica }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_ofe_tipos_contratos_modificar", parametros);
        }

        public void EliminarTipoContrato(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "tipcon_consecutivoP", consecutivo}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_ofe_tipos_contratos_eliminar", parametros);
        }
        #endregion

        #region Parámetros Evaluación Prácticas

        public IList<ParametroEvaluacionProgramasPracticasViewModel> ObtenerParametrosEvaluacionProgramasPracticas(string texto, bool? estado, string programa, int? padre)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_texto", texto },
                { "filtros_estado", estado },
                { "pro_codigoP", programa },
                { "parevapropra_padre", padre }
            };

            return institucion.BaseDatos.ExecuteQuery<ParametroEvaluacionProgramasPracticasViewModel>("pa_aca_parametros_evaluacion_programas_practicas_retornar", parametros);
        }

        public ParametroEvaluacionProgramasPracticasViewModel ObtenerDetalleParametroEvaluacionProgramasPractica(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "parevapropra_consecutivoP", consecutivo }
            };

            return institucion.BaseDatos.ExecuteQuery<ParametroEvaluacionProgramasPracticasViewModel>("pa_aca_parametros_evaluacion_programas_practicas_detalle_retornar", parametros).FirstOrDefault();
        }

        public void IngresarParametroEvaluacionProgramaPractica(string programa, string nombre, bool estado, int? padre, int? orden, bool parametroPrioritario)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "parevapropra_nombre", nombre },
                { "parevapropra_pro_codigo", programa },
                { "parevapropra_estado", estado },
                { "parevapropra_descripcion", null },
                { "parevapropra_orden", orden },
                { "parevapropra_padre", padre },
                { "parevapropra_prioritario", parametroPrioritario }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_parametros_evaluacion_programas_practicas_ingresar", parametros);
        }

        public void ModificarParametroEvaluacionProgramaPractica(int consecutivo, string nombre, bool estado, bool parametroPrioritario)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "parevapropra_consecutivoP", consecutivo },
                { "parevapropra_nombre", nombre },
                { "parevapropra_estado", estado },
                { "parevapropra_descripcion", null },
                { "parevapropra_prioritario", parametroPrioritario }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_parametros_evaluacion_programas_practicas_modificar", parametros);
        }

        public void EliminarParametroEvaluacionProgramaPractica(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "parevapropra_consecutivoP", consecutivo}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_parametros_evaluacion_programas_practicas_eliminar", parametros);
        }

        public void ReordenarParametros(string consecutivo, int? ordenActual)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "parevapropra_pro_codigo", consecutivo },
                { "parevapropra_orden", ordenActual },
                { "parevapropra_numero", -1 }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_parametros_evaluacion_programas_practicas_orden_modificar", parametros);
        }

        public void OrdenarParametro(int consecutivo, int Orden)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "parevapropra_consecutivoP", consecutivo },
                { "parevapropra_orden", Orden }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_parametro_evaluacion_programas_practicas_orden_modificar", parametros);
        }

        public IList<ParametroEvaluacionAsignaturasModulosPracticasViewModel> ObtenerParametrosEvaluacionModuloAsignaturaPracticas(string texto, bool? estado, string modulo, string asignatura, int? padre)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_texto", texto },
                { "filtros_estado", estado },
                { "mod_codigoP", modulo },
                { "asi_codigoP", asignatura },
                { "parevaasimodpra_padre", padre }
            };

            return institucion.BaseDatos.ExecuteQuery<ParametroEvaluacionAsignaturasModulosPracticasViewModel>("pa_aca_parametros_evaluacion_asignatura_modulo_practicas_retornar", parametros);
        }

        public ParametroEvaluacionAsignaturasModulosPracticasViewModel ObtenerDetalleParametroEvaluacionAsignaturasModulosPractica(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "parevaasimodpra_consecutivoP", consecutivo }
            };

            return institucion.BaseDatos.ExecuteQuery<ParametroEvaluacionAsignaturasModulosPracticasViewModel>("pa_aca_parametros_evaluacion_asignatura_modulo_practicas_detalle_retornar", parametros).FirstOrDefault();
        }

        public void IngresarParametroEvaluacionAsignaturaModuloPractica(string asignatura, string modulo, string nombre, bool estado, int? padre, int? orden, bool parametroPrioritario)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "parevaasimodpra_nombre", nombre },
                { "parevaasimodpra_mod_codigo", modulo },
                { "parevaasimodpra_asi_codigo", asignatura },
                { "parevaasimodpra_estado", estado },
                { "parevaasimodpra_descripcion", null },
                { "parevaasimodpra_orden", orden },
                { "parevaasimodpra_padre", padre },
                { "parevaasimodpra_prioritario", parametroPrioritario }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_parametros_evaluacion_asignatura_modulo_practicas_ingresar", parametros);
        }

        public void ModificarParametroEvaluacionAsignaturaModuloPractica(int consecutivo, string nombre, bool estado, bool parametroPrioritario)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "parevaasimodpra_consecutivoP", consecutivo },
                { "parevaasimodpra_nombre", nombre },
                { "parevaasimodpra_estado", estado },
                { "parevaasimodpra_descripcion", null },
                { "parevaasimodpra_prioritario", parametroPrioritario }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_parametros_evaluacion_asignatura_modulo_practicas_modificar", parametros);
        }

        public void EliminarParametroEvaluacionAsignaturaModuloPractica(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "parevaasimodpra_consecutivoP", consecutivo}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_parametros_evaluacion_asignatura_modulo_practicas_eliminar", parametros);
        }

        public void ReordenarParametrosAsignaturaModulo(string asignatura, string modulo, int? ordenActual)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "parevaasimodpra_asi_codigo", asignatura },
                { "parevaasimodpra_mod_codigo", modulo },
                { "parevaasimodpra_orden", ordenActual },
                { "parevaasimodpra_numero", -1 }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_parametros_evaluacion_asignatura_modulo_practicas_orden_modificar", parametros);
        }

        public void OrdenarParametroAsignaturaModulo(int consecutivo, int Orden)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "parevaasimodpra_consecutivoP", consecutivo },
                { "parevaasimodpra_orden", Orden }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_parametro_evaluacion_asignatura_modulo_practicas_orden_modificar", parametros);
        }
        #endregion

        #region Cierre Cursos Asignaturas

        public int ValidarCursoCerrarArchivar(int? consecutivo)
        {
            var parametros = new Dictionary<string, object>(){
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaaca_consecutivoP", consecutivo}
            };

            return institucion.BaseDatos.ExecuteQuery<dynamic>("pa_aca_planes_academicos_validar_para_cerrar_retornar", parametros).FirstOrDefault().sepuedecerrarplan;
        }

        public bool ValidarEstudianteInactivosCurso(int? consecutivo)
        {
            var parametros = new Dictionary<string, object>(){
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaaca_consecutivoP", consecutivo}
            };

            return Convert.ToBoolean(institucion.BaseDatos.ExecuteQuery<dynamic>("pa_aca_matriculas_planes_estudiantes_inactivos_validar", parametros).FirstOrDefault().sepuedecerrarplan);
        }

        public void EliminarEstudiantesInactivosCurso(int? consecutivo)
        {
            var parametros = new Dictionary<string, object>(){
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaaca_consecutivoP", consecutivo },
                { "aud_usuario_sesion", per_codigoP },
                { "aud_ip",ClientIpAddress },
                { "aud_cat_codigo", CategoriaAuditoria },
                { "aud_usuario_q10", codigo_ejecutivo_institucion },
                { "aud_interno", false }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_matriculas_planes_inactivos_eliminar", parametros);
        }

        public void CerrarArchivarCurso(int consecutivo, bool cerrado, bool archivado, int commandTimeOut = 180)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaaca_consecutivoP", consecutivo },
                { "plaaca_cerrado", cerrado },
                { "plaaca_archivado", archivado },
                { "aud_usuario_sesion", per_codigoP },
                { "aud_ip",ClientIpAddress },
                { "aud_cat_codigo", CategoriaAuditoria },
                { "aud_usuario_q10", codigo_ejecutivo_institucion },
                { "aud_interno", false },
            };

            if (institucion.ValidarPersonalizacion(Constantes.ZPERSONALIZACION_SALLE))
                parametros.Add("personalizaciones", Constantes.ZPERSONALIZACION_SALLE);

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_planes_academicos_trabajo_superior_cerrar_archivar", parametros, commandTimeout: commandTimeOut);
        }
        public void PrematricularEstudiantesCursoClonado(string cursos)
        {
            var parametros = new Dictionary<string, object>(){
                {"aplent_codigoP",institucion.aplent_codigoP },
                {"plaaca_consecutivos", cursos}
            };
            institucion.BaseDatos.ExecuteNonQuery("pa_aca_planes_academicos_clonados_prematricular", parametros);
        }
        public void CopiarCursos(int consecutivo, DateTime? fechainicio, DateTime? fechafin, int? periodo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaaca_consecutivoP", consecutivo },
                { "plaaca_fecha_inicio", fechainicio },
                { "plaaca_fecha_fin", fechafin },
                { "per_consecutivoP", periodo }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_planes_academicos_copiar", parametros);
        }

        #endregion

        #region Periodos Historicos

        public IList<PeriodoViewModel> ObtenerPeriodosHistoricos(string codigoMatricula, int? pensum)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "mat_codigoP", codigoMatricula },
                { "propen_consecutivo", pensum }
            };

            return institucion.BaseDatos.ExecuteQuery<Q10.Jack.Areas.Establecimiento.ViewModels.Periodos.PeriodoViewModel>("pa_par_periodos_historicos_retornar", parametros);
        }

        #endregion

        #region Tipos Ingreso

        public IList<TiposIngresosViewModel> ObtenerTiposIngreso(bool? estado, string texto)
        {
            var parametros = new Dictionary<string, object>(){
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_estado", estado },
                { "filtros_texto", texto }
            };
            return institucion.BaseDatos.ExecuteQuery<TiposIngresosViewModel>("pa_aca_tipos_ingreso_retornar", parametros);
        }

        public TiposIngresosViewModel ObtenerDetalleTiposIngreso(int tiping_consecutivoP)
        {
            var parametros = new Dictionary<string, object>(){
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "tiping_consecutivoP", tiping_consecutivoP}
            };
            return institucion.BaseDatos.ExecuteQuery<TiposIngresosViewModel>("pa_aca_tipos_ingreso_detalle_retornar", parametros).FirstOrDefault();
        }

        public IList<TiposIngresosViewModel> IngresarTiposIngreso(string tiping_nombre, bool? estado)
        {
            var parametros = new Dictionary<string, object>(){
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "tiping_nombre", tiping_nombre },
                { "tiping_estado ", estado}
            };
            return institucion.BaseDatos.ExecuteQuery<TiposIngresosViewModel>("pa_aca_tipos_ingreso_ingresar", parametros);
        }

        public IList<TiposIngresosViewModel> modificarTiposIngreso(int tiping_consecutivoP, string tiping_nombre, bool? estado)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "tiping_consecutivoP", tiping_consecutivoP },
                { "tiping_nombre", tiping_nombre },
                { "tiping_estado ", estado}
            };
            return institucion.BaseDatos.ExecuteQuery<TiposIngresosViewModel>("pa_aca_tipos_ingreso_modificar", parametros);
        }

        public void eliminarTiposIngreso(int tiping_consecutivoP)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "tiping_consecutivoP", tiping_consecutivoP}
            };
            institucion.BaseDatos.ExecuteQuery<TiposIngresosViewModel>("pa_aca_tipos_ingreso_eliminar", parametros);
        }

        #endregion

        #region Tipos Seguimientos

        public IList<TiposSeguimientosViewModel> ObtenerTiposSeguimientos(bool? estado, string texto)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtro_estado", estado },
                { "filtro_texto", texto}
            };

            return institucion.BaseDatos.ExecuteQuery<TiposSeguimientosViewModel>("pa_aca_tipos_seguimientos_retornar", parametros);
        }

        public TiposSeguimientosViewModel ObtenerDetalleTiposSeguimientos(int codigo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "tipseg_consecutivoP", codigo }
            };

            return institucion.BaseDatos.ExecuteQuery<TiposSeguimientosViewModel>("pa_aca_tipos_seguimientos_detalle_retornar", parametros).FirstOrDefault();
        }

        public void IngresarTipo(bool estado, string nombre, decimal? porcentaje)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "tipseg_estado", estado },
                { "tipseg_nombre", nombre },
                { "tipseg_porcentaje", porcentaje}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_tipos_seguimientos_ingresar", parametros);
        }

        public void EliminarTipo(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "tipseg_consecutivoP", consecutivo}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_tipos_seguimientos_eliminar", parametros);
        }

        public void ModificarTipo(int codigo, bool estado, string titulo, decimal? porcentaje)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "tipseg_consecutivoP", codigo },
                { "tipseg_nombre", titulo },
                { "tipseg_estado", estado },
                { "tipseg_porcentaje", porcentaje}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_tipos_seguimientos_modificar", parametros);
        }

        #endregion

        #region Asignaturas Equivalentes

        /// <summary>
        /// Cuando no se le pasa el consecutivo de pénsum, valida que la asignatura a agregar no esté en el mismo pénsum de la actual.
        /// Cuando se le pasa el consecutivo del pénsum, valida que en las asignaturas actuales no exista alguna que sea equivalente a la que voy a agregar.
        /// </summary>
        /// <param name="codigoAsignatura"></param>
        /// <param name="codigoAsignaturaEquivalente"></param>
        /// <param name="consecutivoPensum"></param>
        /// <returns>1 para permitir, 0 para no permitir.</returns>
        public bool ValidarAsignaturasEquivalentesPensum(string codigoAsignatura, string codigoAsignaturaEquivalente, int? consecutivoPensum = null)
        {
            var parameters = new Dictionary<string, object>()
            {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "asi_codigoP", codigoAsignatura },
                { "asiequi_asi_codigo", codigoAsignaturaEquivalente },
                { "propen_consecutivoP", consecutivoPensum},
            };

            return Convert.ToBoolean(institucion.BaseDatos.ExecuteNonQuery("pa_aca_asignaturas_pensum_validar", parameters));
        }

        public IList<AsignaturaequivalentesViewModel> ObtenerAsignaturasEquivalentes(string idAsignatura)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "asi_codigoP", idAsignatura }
            };

            return institucion.BaseDatos.ExecuteQuery<AsignaturaequivalentesViewModel>("pa_aca_asignaturas_equivalencias_retornar", parametros);
        }

        public async Task<IList<AsignaturaequivalentesViewModel>> ObtenerAsignaturasEquivalentesAsync(string idAsignatura)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "asi_codigoP", idAsignatura }
            };

            return await institucion.BaseDatos.ExecuteQueryAsync<AsignaturaequivalentesViewModel>("pa_aca_asignaturas_equivalencias_retornar", parametros);
        }

        public AsignaturaequivalentesViewModel ObtenerDetalleAsignaturaEquivalente(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "asiequ_consecutivoP", consecutivo }
            };

            return institucion.BaseDatos.ExecuteQuery<AsignaturaequivalentesViewModel>("pa_aca_asignaturas_equivalencias_detalle_retornar", parametros).FirstOrDefault();
        }

        public void IngresarAsignaturaEquivalente(string codigoAsignatura, string codigoAsignaturaEquivalente)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "asiequ_asi_codigo1", codigoAsignatura },
                { "asiequ_asi_codigo2", codigoAsignaturaEquivalente },
                { "aud_usuario_sesion", per_codigoP },
                { "aud_ip",ClientIpAddress },
                { "aud_cat_codigo", CategoriaAuditoria },
                { "aud_usuario_q10", codigo_ejecutivo_institucion },
                { "aud_interno", false }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_asignaturas_equivalencias_ingresar", parametros);
        }

        public void EliminarAsignaturaEquivalente(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "asiequ_consecutivoP", consecutivo },
                //Modifcación en auditoria                
                { "aud_usuario_sesion", per_codigoP },
                { "aud_ip",ClientIpAddress },
                { "aud_cat_codigo", CategoriaAuditoria },
                { "aud_usuario_q10", codigo_ejecutivo_institucion },
                { "aud_interno", false }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_asignaturas_equivalencias_eliminar", parametros);
        }

        #endregion

        #region Tipos Cancelacion

        public IList<TiposCancelacionViewModel> ObtenerTiposCancelacion(string texto, bool? estado)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_texto", texto },
                { "filtro_estado", estado }
            };

            return institucion.BaseDatos.ExecuteQuery<TiposCancelacionViewModel>("pa_aca_tipo_cancelaciones_retornar", parametros);
        }

        public TiposCancelacionViewModel ObtenerDetalleTipoCancelacion(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "tipcan_consecutivoP", consecutivo }
            };

            return institucion.BaseDatos.ExecuteQuery<TiposCancelacionViewModel>("pa_aca_tipo_cancelaciones_detalle_retornar", parametros).FirstOrDefault();
        }

        public void IngresarTipoCancelacion(string descripcion, bool estado)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "tipcan_descripcion", descripcion },
                { "tipcan_estado", estado }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_tipo_cancelaciones_ingresar", parametros);
        }

        public void ModificarTipoCancelacion(int consecutivo, string descripcion, bool estado)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "tipcan_consecutivoP", consecutivo },
                { "tipcan_descripcion", descripcion },
                { "tipcan_estado", estado }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_tipo_cancelaciones_modificar", parametros);
        }

        public void EliminarTipoCancelacion(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "tipcan_consecutivoP", consecutivo}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_tipo_cancelaciones_eliminar", parametros);
        }

        #region API - Tipos Cancelacion

        public async Task<IPagedList<TiposCancelacionViewModel>> ObtenerTiposCancelacionApiAsync(string texto, bool? estado, int pagina = 1, int tamanioPagina = Constantes.PAGINACION_TAMANO_PAGINA)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_texto", texto },
                { "filtro_estado", estado }
            };

            var result = await institucion.BaseDatos.ExecuteQueryAsync<TiposCancelacionViewModel>("pa_aca_tipo_cancelaciones_retornar", parametros, pagina, tamanioPagina);
            return new StaticPagedList<TiposCancelacionViewModel>(result.Item1, pagina, tamanioPagina, result.Item2);
        }

        #endregion API - Tipos Cancelacion

        #endregion

        #region Causas Cancelacion

        public IList<CausasCancelacionViewModel> ObtenerCausasCancelacion(string texto, bool? estado)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_texto", texto },
                { "filtro_estado", estado }
            };

            return institucion.BaseDatos.ExecuteQuery<CausasCancelacionViewModel>("pa_aca_causas_retornar", parametros);
        }

        public CausasCancelacionViewModel ObtenerDetalleCausaCancelacion(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "cau_consecutivoP", consecutivo }
            };

            return institucion.BaseDatos.ExecuteQuery<CausasCancelacionViewModel>("pa_aca_causas_detalle_retornar", parametros).FirstOrDefault();
        }

        public void IngresarCausaCancelacion(string descripcion, bool estado)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "cau_nombre", descripcion },
                { "cau_estado", estado }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_causas_ingresar", parametros);
        }

        public void ModificarCausaCancelacion(int consecutivo, string descripcion, bool estado)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "cau_consecutivoP", consecutivo },
                { "cau_nombre", descripcion },
                { "cau_estado", estado }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_causas_modificar", parametros);
        }

        public void EliminarCausaCancelacion(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "cau_consecutivoP", consecutivo}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_causas_eliminar", parametros);
        }

        #region API Causas cancelación

        public async Task<IPagedList<CausasCancelacionViewModel>> ObtenerCausasCancelacionAPIAsync(bool estado, int pagina = 1, int tamanioPagina = Constantes.PAGINACION_TAMANO_PAGINA)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_texto", null },
                { "filtro_estado", estado }
            };

            var result = await institucion.BaseDatos.ExecuteQueryAsync<CausasCancelacionViewModel>("pa_aca_causas_retornar", parametros, pagina, tamanioPagina);
            return new StaticPagedList<CausasCancelacionViewModel>(result.Item1, pagina, tamanioPagina, result.Item2);
        }

        #endregion API Causas cancelación

        #endregion

        #region reporte nómina matrícula

        public List<NominaMatriculaViewModel> ObtenerInformeNominaMatricula(int grupo, int? consecutivogruren, int filtro)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gru_consecutivoP", grupo },
                { "gruren_consecutivoP", consecutivogruren },
                { "filtro_tipo", filtro}
            };
            return institucion.BaseDatos.ExecuteQuery<NominaMatriculaViewModel>("pa_inf_aca_excel_nomina_matriculas", parametros).ToList();
        }

        public List<ActaConsolidadaViewModel> ObtenerInformacionActaConsolidadaModular(int grupo, int? consecutivogruren, int? filtro = 1)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gru_consecutivoP", grupo },
                { "gruren_consecutivoP", consecutivogruren },
                { "filtro_tipos", filtro}
            };
            return institucion.BaseDatos.ExecuteQuery<ActaConsolidadaViewModel>("pa_inf_aca_excel_acta_consolidado_modular", parametros).ToList();
        }

        public List<ActaConsolidadaViewModel> ObtenerInformacionActaConsolidadaCuantitativo(int grupo, int? consecutivogruren, int? filtro = 1)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gru_consecutivoP", grupo },
                { "gruren_consecutivoP", consecutivogruren },
                { "filtro_tipos", filtro}
            };
            return institucion.BaseDatos.ExecuteQuery<ActaConsolidadaViewModel>("pa_inf_aca_excel_acta_consolidado_cuantitativo", parametros).ToList();
        }

        public List<ActaUnidadDidacticaViewModel> ObtenerInformacionActaUnidadDidactica(int? consecutivogruren, int? curso = null)
        {
            var parametros = new Dictionary<string, object>(){
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gruren_consecutivoP", consecutivogruren },
                { "plaaca_consecutivoP", curso}
            };

            return institucion.BaseDatos.ExecuteQuery<ActaUnidadDidacticaViewModel>("paz_amazonia_inf_aca_acta_evaluacion_unidad_didactica", parametros).ToList();
        }

        #endregion

        #region Personalizaciones

        public IList<GruposAdmisionViewModel> ObtenerGruposAdmision(int? periodo, int? grado, string programa, int? sedjornada, DateTime? fecha_inicio = null, DateTime? fecha_fin = null, string docente = null, int? sedeJornadaPrograma = null, string jurado = null, int? categoria = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gra_consecutivoP", grado },
                { "per_consecutivoP", periodo },
                { "sedjor_consecutivoP", sedjornada },
                { "filtro_fecha_inicio", fecha_inicio},
                { "filtro_fecha_fin", fecha_fin},
                { "gruadm_per_codigo_docente", docente},
                { "pro_codigoP", programa },
                { "gruadm_admcat_consecutivo", categoria },
                { "sedjorprogra_consecutivoP", sedeJornadaPrograma },
                { "gruadmjur_per_codigo", jurado },
            };

            return institucion.BaseDatos.ExecuteQuery<GruposAdmisionViewModel>("pa_aca_grupos_admisiones_retornar", parametros);
        }

        public GruposAdmisionViewModel ObtenerDetalleGrupoAdmision(int? consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gruadm_consecutivoP", consecutivo }
            };

            return institucion.BaseDatos.ExecuteQuery<GruposAdmisionViewModel>("pa_aca_grupos_admisiones_detalle_retornar", parametros).FirstOrDefault();
        }

        public bool ValidarJurado(int consecutivoGrupo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gruadm_consecutivoP", consecutivoGrupo },
            };

            return Convert.ToBoolean(institucion.BaseDatos.ExecuteNonQuery("pa_per_admisiones_categorias_examen_por_jurado_validar", parametros));
        }

        public int IngresarGrupoAdmision(string nombre, int? sedjor, int? aula, DateTime? fecha, TimeSpan? horaInicio, TimeSpan? horaFin, int? periodo, string docente, int? cupoMaximo, int? categoria = null, string parametrosCategoria = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gruadm_nombre", nombre },
                { "gruadm_sedjorprogra_consecutivo", sedjor },
                { "gruadm_aul_consecutivo", aula },
                { "gruadm_fecha", fecha },
                { "gruadm_hora_inicio",horaInicio },
                { "gruadm_hora_fin", horaFin },
                { "gruadm_per_consecutivo", periodo },
                { "gruadm_per_codigo_docente", docente },
                { "gruadm_cupo_maximo" , cupoMaximo },
                //Campo personalizado UNM
                { "gruadm_admcat_consecutivo", categoria },
                { "gruadm_admcatpar_consecutivos", parametrosCategoria}
            };

            return institucion.BaseDatos.ExecuteNonQuery("pa_aca_grupos_admisiones_ingresar", parametros);
        }

        public void ModificarGrupoAdmision(string nombre, int? consecutivo, DateTime? fecha, int? aula, TimeSpan? horaInicio, TimeSpan? horaFin, string docente, int? cupoMaximo, int? categoria = null, string parametrosCategoria = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gruadm_nombre", nombre },
                { "gruadm_consecutivoP", consecutivo },
                { "gruadm_fecha" , fecha },
                { "gruadm_aul_consecutivo", aula },
                { "gruadm_hora_inicio",horaInicio },
                { "gruadm_hora_fin", horaFin },
                { "gruadm_per_codigo_docente", docente },
                { "gruadm_cupo_maximo" , cupoMaximo },
                //Campo personalizado UNM
                { "gruadm_admcat_consecutivo", categoria },
                { "gruadm_admcatpar_consecutivos", parametrosCategoria},
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_grupos_admisiones_modificar", parametros);
        }

        public void EliminarGrupoAdmisio(int? consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gruadm_consecutivoP", consecutivo}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_grupos_admisiones_eliminar", parametros);
        }

        public bool ValidarCruceHorarioGruposAdmision(int? grupo, int? aula, string fecha, TimeSpan? inicio, TimeSpan? fin, int? periodo, string docente)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gruadm_consecutivoP", grupo },
                { "gruadm_aul_consecutivo", aula },
                { "gruadm_fecha", fecha },
                { "gruadm_hora_inicio", inicio },
                { "gruadm_hora_fin", fin },
                { "gruadm_per_consecutivo", periodo },
                { "gruadm_per_codigo_docente", docente}
            };

            return Convert.ToBoolean(institucion.BaseDatos.ExecuteNonQuery("pa_aca_grupos_admisiones_cruce_horario_validar", parametros));
        }

        public IList<EstudiantesGruposAdmisionViewModel> ObtenerEstudiantesGrupoAdmision(int? consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gruadm_consecutivoP", consecutivo },
            };

            return institucion.BaseDatos.ExecuteQuery<EstudiantesGruposAdmisionViewModel>("pa_aca_grupos_admisiones_estudiantes_retornar", parametros);
        }

        public void EliminarEstudiantesGrupoAdmision(int? consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gruadmest_consecutivoP", consecutivo },
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_grupos_admisiones_estudiantes_eliminar", parametros);
        }

        public IList<EstudiantesGruposAdmisionViewModel> ObtenerEstudiantesParaAsignarGrupoAdmision(int? gra_consecutivoP, int? per_consecutivoP, int? gruadm_consecutivoP, string programa, int tipo = 0, int? sedeJornadaPrograma = null, int? categoria = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gra_consecutivoP", gra_consecutivoP },
                { "per_consecutivoP", per_consecutivoP },
                { "gruadm_consecutivoP", gruadm_consecutivoP },
                { "pro_codigoP", programa },
                { "sedjorprogra_consecutivo", sedeJornadaPrograma },
                { "filtro_tipo", tipo },
                { "admcat_actual", categoria}
            };

            return institucion.BaseDatos.ExecuteQuery<EstudiantesGruposAdmisionViewModel>("pa_aca_grupos_admisiones_estudiantes_para_asignar_retornar", parametros);
        }

        public void IngresarEstudianteGrupoAdmision(int? grado, string codigoP, int inscripcion)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gruadmest_gruadm_consecutivo", grado },
                { "gruadmest_per_codigo", codigoP },
                { "gruadmest_ins_consecutivo", inscripcion }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_grupos_admisiones_estudiantes_ingresar", parametros);
        }

        public void ModificarEstudianteGrupoAdmision(int? consecutivoDetalle, string nivel)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gruadmest_consecutivoP", consecutivoDetalle },
                { "gruadmest_niv_codigo", nivel }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_grupos_admisiones_estudiantes_nivel_modificar", parametros);
        }

        public EstudiantesGruposAdmisionViewModel ObtenerDetalleEstudianteGrupoAdmision(int? grupo, string codigoP, int inscripcion)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gruadmest_consecutivoP", grupo },
                { "gruadmest_per_codigo", codigoP },
                { "gruadmest_ins_consecutivo", inscripcion }
            };

            return institucion.BaseDatos.ExecuteQuery<EstudiantesGruposAdmisionViewModel>("pa_aca_grupos_admisiones_estudiantes_detalle_retornar", parametros).FirstOrDefault();
        }

        public bool ValidarGradoAplicaEvaluacionTransicion(int consecutivoGrado, int periodo)
        {
            var parameters = new Dictionary<string, object>()
            {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "gra_consecutivoP ", consecutivoGrado },
                { "per_consecutivo ", periodo }
            };

            var result = institucion.BaseDatos.ExecuteQuery<dynamic>("pa_aca_grados_evaluacion_transicion_validar", parameters).FirstOrDefault();
            return Convert.ToBoolean(result.Aplica);
        }

        #region Cupos Programa Zona Cinco

        public IList<CupoProgramaViewModel> ObtenerCuposProgramas(string programa, int? periodo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "proper_pro_codigo", programa },
                { "proper_per_consecutivo", periodo}
            };

            return institucion.BaseDatos.ExecuteQuery<CupoProgramaViewModel>("paz_zona_cinco_aca_programas_periodos_retornar", parametros);
        }

        public CupoProgramaViewModel ObtenerDetalleCupoPrograma(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "proper_consecutivoP", consecutivo }
            };

            return institucion.BaseDatos.ExecuteQuery<CupoProgramaViewModel>("paz_zona_cinco_aca_programas_periodos_detalle_retornar", parametros).FirstOrDefault();
        }

        public void IngresarCupoPrograma(string programa, int? periodo, int? cupo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "proper_pro_codigo", programa },
                { "proper_per_consecutivo", periodo },
                { "proper_cupo_maximo", cupo },
            };

            institucion.BaseDatos.ExecuteNonQuery("paz_zona_cinco_aca_programas_periodos_ingresar", parametros);
        }

        public void ModificarCupoPrograma(int consecutivo, int? cupo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "proper_consecutivoP", consecutivo },
                { "proper_cupo_maximo", cupo },
            };

            institucion.BaseDatos.ExecuteNonQuery("paz_zona_cinco_aca_programas_periodos_modificar", parametros);
        }

        public void EliminarCupoPrograma(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "proper_consecutivoP", consecutivo }
            };

            institucion.BaseDatos.ExecuteNonQuery("paz_zona_cinco_aca_programas_periodos_eliminar", parametros);
        }

        public int? ValidarCupoProgramaDisponible(string programa, int? periodo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "proper_pro_codigo", programa },
                { "proper_per_consecutivo", periodo }
            };
            var modelo = institucion.BaseDatos.ExecuteQuery<CupoProgramaViewModel>("paz_zona_cinco_aca_cupos_disponibles_retornar", parametros).FirstOrDefault();
            var cupo = modelo != null && modelo.proper_cupo_maximo.HasValue ? modelo.proper_cupo_disponible : new Nullable<int>();
            return cupo;
        }

        #endregion Cupos Programa Zona Cinco

        #region Personalizacion Amazonía

        public IList<FacultadesAmazoniaViewModel> ObtenerFacultadesAmazonia(bool? estado, string texto)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_estado", estado },
                { "filtros_texto", texto },
            };

            return institucion.BaseDatos.ExecuteQuery<FacultadesAmazoniaViewModel>("pa_par_facultades_retornar", parametros);
        }

        public FacultadesAmazoniaViewModel ObtenerDetalleFacultadAmazonia(string codigo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "fac_codigoP", codigo },
            };

            return institucion.BaseDatos.ExecuteQuery<FacultadesAmazoniaViewModel>("pa_par_facultades_detalle_retornar", parametros).FirstOrDefault();
        }

        public void IngresarFacultadAmazonia(string codigo, string nombre, bool estado)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "fac_codigoP", codigo },
                { "fac_nombre", nombre },
                { "fac_estado", estado },
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_par_facultades_ingresar", parametros);
        }

        public void ModificarFacultadAmazonia(string codigo, string nombre, bool estado)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "fac_codigoP", codigo },
                { "fac_nombre", nombre },
                { "fac_estado", estado },
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_par_facultades_modificar", parametros);
        }

        public void EliminarFacultadAmazonia(string codigo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "fac_codigoP", codigo },
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_par_facultades_eliminar", parametros);
        }

        public IList<AdministrarDocumentoViewModel> ObtenerDocumentosAsignatura(int? pensum, string asignatura, int? periodo = null)
        {
            //orden A: Asignatura, N: Nivel
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "docrec_propen_consecutivo", pensum },
                { "docrec_asi_codigo", asignatura },
                { "docrec_per_consecutivo", periodo },
                { "per_codigoP", usuario.per_codigoP },
                { "rol_codigoP", usuario.rolper_rol_codigo },
            };

            return institucion.BaseDatos.ExecuteQuery<AdministrarDocumentoViewModel>("paz_amazonia_aca_documentos_relacionados_retornar", parametros);
        }

        public void EliminarDocumentoAsignatura(string idAsignatura, int idDocumeto, int programaPensum, int periodo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "docrec_arc_consecutivo", idDocumeto },
                { "docrec_propen_consecutivo", programaPensum },
                { "docrec_asi_codigo", idAsignatura },
                { "docrec_per_consecutivo", periodo },
            };

            institucion.BaseDatos.ExecuteNonQuery("paz_amazonia_aca_documentos_relacionados_eliminar", parametros);
        }

        public void IngresarDocumentoAsignatura(string idAsignatura, int idDocumeto, int programaPensum, int periodo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "docrec_arc_consecutivo", idDocumeto },
                { "docrec_propen_consecutivo", programaPensum },
                { "docrec_asi_codigo", idAsignatura },
                { "docrec_per_consecutivo", periodo },
            };

            institucion.BaseDatos.ExecuteNonQuery("paz_amazonia_aca_documentos_relacionados_ingresar", parametros);
        }

        #endregion

        #region Personalizaciones Escuela Británica

        #region Asignaturas - Lecciones

        public IList<LeccionViewModel> ObtenerLeccionesAsignaturasIndividuales(string leccion, string filtro_texto, string asignatura)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "lecasi_codigoP", leccion },
                { "filtro_texto", filtro_texto },
                { "lecasi_asi_codigo", asignatura }
            };

            return institucion.BaseDatos.ExecuteQuery<LeccionViewModel>("paz_britanica_aca_lecciones_asignaturas_retornar", parametros).ToList();
        }

        public LeccionViewModel ObtenerDetalleLeccionAsignaturasIndividuales(string leccion)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "lecasi_codigoP", leccion }
            };

            return institucion.BaseDatos.ExecuteQuery<LeccionViewModel>("paz_britanica_aca_lecciones_asignaturas_detalle_retornar", parametros).FirstOrDefault();
        }

        public void IngresarLeccionAsignaturasIndividuales(string codigo, string nombre, string abreviacion, bool estado, int orden, string asignatura)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "lecasi_codigoP", codigo },
                { "lecasi_nombre", nombre },
                { "lecasi_abreviacion", abreviacion },
                { "lecasi_estado", estado },
                { "lecasi_orden", orden },
                { "lecasi_asi_codigo", asignatura }
            };

            institucion.BaseDatos.ExecuteNonQuery("paz_britanica_aca_lecciones_asignaturas_ingresar", parametros);
        }

        public void IngresarRelacionLeccionHorario(int horario, string leccion)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "lecasi_codigoP", leccion },
                { "lecasi_matplahor_consecutivo", horario },
            };

            institucion.BaseDatos.ExecuteNonQuery("paz_britanica_aca_lecciones_asignaturas_ingresar", parametros);
        }

        public void ModificarLeccionAsignaturasIndividuales(string codigo, string nombre, string abreviacion, bool estado, int orden)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "lecasi_codigoP", codigo },
                { "lecasi_nombre", nombre },
                { "lecasi_abreviacion", abreviacion },
                { "lecasi_estado", estado },
                { "lecasi_orden", orden }
            };

            institucion.BaseDatos.ExecuteNonQuery("paz_britanica_aca_lecciones_asignaturas_modificar", parametros);
        }

        public void EliminarLeccionAsignaturasIndividuales(string leccion)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "lecasi_codigoP", leccion }
            };

            institucion.BaseDatos.ExecuteNonQuery("paz_britanica_aca_lecciones_asignaturas_eliminar", parametros);
        }

        public LeccionViewModel ObtenerProximaLeccionAsignatura(int matPlan)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "matpla_consecutivoP", matPlan }
            };

            return institucion.BaseDatos.ExecuteQuery<LeccionViewModel>("paz_britanica_aca_proxima_lecciones_retornar", parametros).FirstOrDefault();
        }

        #endregion Asignaturas - Lecciones 

        #region Asignaturas Horarios

        public List<HorarioViewModel> ObtenerAsignaturasMatriculas(int horario, string asignatura)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "hor_consecutivoP", horario },
                { "asi_codigoP", asignatura }
            };

            return institucion.BaseDatos.ExecuteQuery<HorarioViewModel>("pa_aca_matriculas_planes_horarios_asignatura_validar", parametros).ToList();
        }

        public IList<HorarioViewModel> ObtenerAsignaturasHorarios(int horario, string asignatura)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "horasi_hor_consecutivo", horario },
                { "horasi_asi_codigo", asignatura }
            };

            return institucion.BaseDatos.ExecuteQuery<HorarioViewModel>("pa_aca_horarios_asignaturas_retornar", parametros).ToList();
        }

        public HorarioViewModel ObtenerDetalleAsignaturaHorario(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "horasi_consecutivoP", consecutivo }
            };

            return institucion.BaseDatos.ExecuteQuery<HorarioViewModel>("pa_aca_horarios_asignaturas_detalle_retornar", parametros).FirstOrDefault();
        }

        public void IngresarAsignaturasHorario(int horario, string asignatura)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "horasi_hor_consecutivo", horario },
                { "horasi_asi_codigo", asignatura }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_horarios_asignaturas_ingresar", parametros);
        }

        public void EliminarAsignaturaHorario(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "horasi_consecutivoP", consecutivo }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_horarios_asignaturas_eliminar", parametros);
        }

        #endregion Asignaturas Horarios

        #endregion Personalizaciones Escuela Británica

        #region Personalizacion Interactuar

        public InformacionProgramaViewModel ObtenerInformacionAdicionalPrograma(string codigo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "pro_codigoP", codigo }
            };

            return institucion.BaseDatos.ExecuteQuery<InformacionProgramaViewModel>("paz_interactuar_aca_programas_informacion_adicional_retornar", parametros).FirstOrDefault();
        }

        public void ModificarInformacionAdicionalPrograma(string codigo, int? lineaTematica, bool procesoSeleccion, string dirigido, string descripcion, string alcance, string IndicadoresLogro, int? numeroConsultores, string beneficiario, string perfilConsultor, int? duracionServicio, int? cicloServicio, bool seguimientoPosterior, int? tiempoEspera, int? duracionSeguimiento, char? tipoSeguimiento)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "pro_codigoP", codigo },
                { "proinfadi_lintem_consecutivo", lineaTematica },
                { "proinfadi_requiere_proceso_seleccion", procesoSeleccion },
                { "proinfadi_dirigido_a", dirigido },
                { "proinfadi_descripcion", descripcion },
                { "proinfadi_alcance", alcance },
                { "proinfadi_beneficio", beneficiario },
                { "proinfadi_indicadores_logro_servicio", IndicadoresLogro },
                { "proinfadi_numero_consultores", numeroConsultores },
                { "proinfadi_perfil_consultores", perfilConsultor },
                { "proinfadi_duracion_servicio", duracionServicio },
                { "proinfadi_ciclo_servicio", cicloServicio },
                { "proinfadi_requiere_seguimiento_posterior", seguimientoPosterior },
                { "proinfadi_tiempo_espera_seguimiento", tiempoEspera },
                { "proinfadi_duracion_seguimiento", duracionSeguimiento },
                { "proinfadi_tipo_seguimiento", tipoSeguimiento }
            };

            institucion.BaseDatos.ExecuteNonQuery("paz_interactuar_aca_programas_informacion_adicional_ingresar_modificar", parametros);
        }

        public InformacionAsignaturaViewModel ObtenerInformacionAdicionalAsignaturaInteractuar(string codigo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "asi_codigoP", codigo }
            };

            return institucion.BaseDatos.ExecuteQuery<InformacionAsignaturaViewModel>("paz_interactuar_aca_asignaturas_informacion_adicional_retornar", parametros).FirstOrDefault();
        }

        public void ModificarInformacionAdicionalAsignaturaInteractuar(string asi_codigo, string entregable, string temas,
            string perfil, string herramientas, decimal? horasIndirectas, string estructuraTecnica, int? archivo_estructuras, int? tiempoIntervencion,
            string materialMinimo, string memorias, int? archivo_memorias)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "asi_codigoP", asi_codigo },
                { "asiinfadi_entregable", entregable },
                { "asiinfadi_temas_intervencion", temas },
                { "asiinfadi_perfil_consultor", perfil },
                { "asiinfadi_herramientas_metodologicas_especificas", herramientas },
                { "asiinfadi_horas_indirectas_servicio", horasIndirectas },
                { "asiinfadi_estructura_tecnica", estructuraTecnica },
                { "asiinfadi_estructura_tecnica_arc_consecutivo", archivo_estructuras },
                { "asiinfadi_tiempo_maximo_intervencion_sesion", tiempoIntervencion },
                { "asiinfadi_material_minimo_operacion_servicio", materialMinimo },
                { "asiinfadi_memorias", memorias },
                { "asiinfadi_memorias_arc_consecutivo", archivo_memorias }
            };

            institucion.BaseDatos.ExecuteNonQuery("paz_interactuar_aca_asignaturas_informacion_adicional_ingresar_modificar", parametros);
        }

        public List<LineaTematicaViewModel> ObtenerLineasTematicas()
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
            };

            return institucion.BaseDatos.ExecuteQuery<LineaTematicaViewModel>("paz_interactuar_aca_lineas_tematicas_retornar", parametros).ToList();
        }

        #endregion Personalizacion interactuar

        #region Personalizaciones ComfaNorte

        public IList<InfraestructuraViewModel> ObtenerInfraestructurasComfaNorte(bool? estado, string texto)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_texto", texto },
                { "filtro_estado", estado }
            };

            return institucion.BaseDatos.ExecuteQuery<InfraestructuraViewModel>("paz_comfanorte_aca_infraestructuras_retornar", parametros);
        }

        public InfraestructuraViewModel ObtenerDetalleInfraestrucutraComfaNorte(string codigo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "inf_codigoP", codigo }
            };

            return institucion.BaseDatos.ExecuteQuery<InfraestructuraViewModel>("paz_comfanorte_aca_infraestructuras_detalle_retornar", parametros).FirstOrDefault();
        }

        public void IngresarInfraestructuraComfaNorte(string codigoInfraestructura, string nombre, bool estado)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "inf_codigoP", codigoInfraestructura },
                { "inf_nombre", nombre },
                { "inf_estado", estado }
            };

            institucion.BaseDatos.ExecuteNonQuery("paz_comfanorte_aca_infraestructuras_ingresar", parametros);
        }

        public void ModificarInfraestructuraComfaNorte(string codigoInfraestructura, string nombre, bool estado)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "inf_codigoP", codigoInfraestructura },
                { "inf_nombre", nombre },
                { "inf_estado", estado }
            };

            institucion.BaseDatos.ExecuteNonQuery("paz_comfanorte_aca_infraestructuras_modificar", parametros);
        }

        public void EliminarInfraestrucutraComfaNorte(string codigoInfraestructura)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "inf_codigoP", codigoInfraestructura}
            };

            institucion.BaseDatos.ExecuteNonQuery("paz_comfanorte_aca_infraestructuras_eliminar", parametros);
        }

        #endregion Personalizaciones ComfaNorte

        #endregion Personalizaciones

        #region Criterios Entrevista
        public IList<CriterioEntrevistaViewModel> ObtenerCriteriosEntrevista(bool? estado, string texto)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtro_estado", estado },
                { "filtros_texto", texto },
            };

            return institucion.BaseDatos.ExecuteQuery<CriterioEntrevistaViewModel>("pa_per_criterios_evaluacion_entrevista_retornar", parametros);
        }

        public CriterioEntrevistaViewModel ObtenerDetalleCriterioEntrevista(int? codigo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "crievaent_consecutivoP", codigo }
            };

            return institucion.BaseDatos.ExecuteQuery<CriterioEntrevistaViewModel>("pa_per_criterios_evaluacion_entrevista_detalle_retornar", parametros).FirstOrDefault();
        }
        public void IngresarCriterioEntrevista(string nombre, bool? estado)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "crievaent_nombre", nombre },
                { "crievaent_estado", estado }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_per_criterios_evaluacion_entrevista_ingresar", parametros);
        }

        public void ModificarCriterioEntrevista(int? consecutivo, string nombre, bool? estado)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "crievaent_consecutivoP", consecutivo },
                { "crievaent_nombre", nombre },
                { "crievaent_estado", estado },
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_per_criterios_evaluacion_entrevista_modificar", parametros);
        }

        public void EliminarCriterioEntrevista(int? codigo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "crievaent_consecutivoP", codigo }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_per_criterios_evaluacion_entrevista_eliminar", parametros);
        }

        /*Proceso de entrevista*/
        public void IngresarProcesoEntrevista(string id, int? inscripcion, int? consecutivo, string observacion, decimal? nota)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "crievaentest_crievaent_consecutivo", consecutivo },
                { "crievaentest_per_codigo", id },
                { "crievaentest_ins_consecutivo", inscripcion },
                { "crievaentest_observacion", observacion },
                { "crievaentest_nota", nota }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_per_criterios_evaluacion_entrevista_estudiante_ingresar", parametros);
        }

        public IList<CriterioEntrevistaViewModel> ObtenerCriteriosEntrevistaEstudiante(string id, int? inscripcion)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "crievaentest_per_codigo", id },
                { "crievaentest_ins_consecutivo", inscripcion },
            };

            return institucion.BaseDatos.ExecuteQuery<CriterioEntrevistaViewModel>("pa_per_criterios_evaluacion_entrevista_estudiante_retornar", parametros);
        }

        public void ModificarProcesoEntrevista(int? consecutivo, string observacion, decimal? nota)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "crievaentest_consecutivoP", consecutivo },
                { "crievaentest_observacion", observacion },
                { "crievaentest_nota", nota }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_per_criterios_evaluacion_entrevista_estudiante_modificar", parametros);
        }
        #endregion

        #region Descuentos Institucionales

        public async Task<List<DescuentoInstitucionalViewModel>> ObtenerDescuentosInstitucionalesAsync(string texto, DateTime? fechaInicio, DateTime? fechaFin, char? tipo, bool? vencidas, string estudiante, string producto, int? descuento)
        {
            var parameters = new Dictionary<string, object>() {
               { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_texto", texto },
                { "prod_codigoP", producto },
                { "per_consecutivoP", null },
                { "des_consecutivoP", descuento },
                { "filtro_fecha_inicio", fechaInicio },
                { "filtro_fecha_fin", fechaFin },
                { "desins_per_codigo", estudiante },
                { "desins_tipo", tipo },
                { "incluir_vencidas", vencidas },
            };

            return (await institucion.BaseDatos.ExecuteQueryAsync<DescuentoInstitucionalViewModel>("pa_par_descuentos_insitucionales_retornar", parameters)).ToList();
        }

        public List<DescuentoInstitucionalViewModel> ObtenerDescuentosInstitucionales(string texto, DateTime? fechaInicio, DateTime? fechaFin, char? tipo, bool? vencidas, string estudiante, string producto, int? descuento)
        {
            var parameters = new Dictionary<string, object>() {
               { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_texto", texto },
                { "prod_codigoP", producto },
                { "per_consecutivoP", null },
                { "des_consecutivoP", descuento },
                { "filtro_fecha_inicio", fechaInicio },
                { "filtro_fecha_fin", fechaFin },
                { "desins_per_codigo", estudiante },
                { "desins_tipo", tipo },
                { "incluir_vencidas", vencidas },
            };

            return institucion.BaseDatos.ExecuteQuery<DescuentoInstitucionalViewModel>("pa_par_descuentos_insitucionales_retornar", parameters).ToList();
        }

        public IPagedList<DescuentoInstitucionalViewModel> ObtenerDescuentosInstitucionales(string texto, DateTime? fechaInicio, DateTime? fechaFin, char? tipo, bool? vencidas, string estudiante, string producto, int? descuento, int pageindex)
        {
            int totalCount;
            var parameters = new Dictionary<string, object>() {
               { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_texto", texto },
                { "prod_codigoP", producto },
                { "per_consecutivoP", null },
                { "des_consecutivoP", descuento },
                { "filtro_fecha_inicio", fechaInicio },
                { "filtro_fecha_fin", fechaFin },
                { "desins_per_codigo", estudiante },
                { "desins_tipo", tipo },
                { "incluir_vencidas", vencidas },
            };

            var result = institucion.BaseDatos.ExecuteQuery<DescuentoInstitucionalViewModel>("pa_par_descuentos_insitucionales_retornar", parameters, pageindex, Constantes.PAGINACION_TAMANO_PAGINA, out totalCount);
            return new StaticPagedList<DescuentoInstitucionalViewModel>(result, pageindex, Constantes.PAGINACION_TAMANO_PAGINA, totalCount);
        }

        public DescuentoInstitucionalViewModel ObtenerDetalleDescuentoInstitucional(int codigo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "desins_consecutivoP", codigo }
            };

            return institucion.BaseDatos.ExecuteQuery<DescuentoInstitucionalViewModel>("pa_par_descuentos_insitucionales_detalle_retornar", parametros).FirstOrDefault();
        }

        public void IngresarDescuentoInstitucional(string estudiante, DateTime? fechaInicio, DateTime? fechaFin, int descuento, char? tipo, string producto, int? periodo = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "desins_per_codigo", estudiante },
                { "desins_prod_codigo", producto },
                { "desins_des_consecutivo", descuento },
                { "desins_fecha_inicio", fechaInicio },
                { "desins_fecha_fin", fechaFin },
                { "desins_tipo", tipo },
                { "aud_usuario_sesion", per_codigoP },
                { "aud_ip", ClientIpAddress },
                { "aud_cat_codigo", CategoriaAuditoria },
                { "aud_usuario_q10", codigo_ejecutivo_institucion },
                { "aud_interno", false },

                //Campo no necesario
                { "desins_per_consecutivo", null }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_par_descuentos_insitucionales_ingresar", parametros);
        }

        public async Task<int> IngresarDescuentoInstitucionalAPIAsync(string estudiante, DateTime? fechaInicio, DateTime? fechaFin, int descuento, char? tipo, string producto)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "desins_per_codigos", estudiante },
                { "desins_prod_codigo", producto },
                { "desins_des_consecutivo", descuento },
                { "desins_fecha_inicio", fechaInicio },
                { "desins_fecha_fin", fechaFin },
                { "desins_tipo", tipo }
            };

            return await institucion.BaseDatos.ExecuteNonQueryAsync("pa_apis_par_descuentos_institucionales_ingresar", parametros);
        }

        public void ModificarDescuentoInstitucional(int codigo, DateTime? fechaInicio, DateTime? fechaFin, int? periodo = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "desins_consecutivoP", codigo },
                { "desins_per_consecutivo", periodo },
                { "desins_fecha_inicio", fechaInicio },
                { "desins_fecha_fin", fechaFin}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_par_descuentos_insitucionales_modificar", parametros);
        }

        public bool ValidarCruceFechasDescuentoInstitucional(string estudiante, DateTime? fechaInicio, DateTime? fechaFin, int descuento, char? tipo, string producto, int? codigo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "desins_consecutivoP", codigo },
                { "per_codigoP", estudiante },
                { "des_consecutivoP", descuento },
                { "desins_fecha_inicio", fechaInicio },
                { "desins_fecha_fin", fechaFin },
                { "desins_tipo", tipo },
                { "desins_prod_codigo", producto },
            };

            return Convert.ToBoolean(institucion.BaseDatos.ExecuteNonQuery("pa_par_descuentos_insitucionales_conflicto_revisar", parametros));

        }

        public void EliminarDescuentoInstitucional(int codigo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "desins_consecutivoP", codigo}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_par_descuentos_insitucionales_eliminar", parametros);
        }
        #endregion Descuentos Institucionales

        #region Horas Vistas

        public HorarioViewModel ObtenerHorasVistas(string persona, string asignatura)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "per_codigoP", persona },
                { "asi_codigoP", asignatura }
            };

            return institucion.BaseDatos.ExecuteQuery<HorarioViewModel>("paz_ciu_aca_horas_asignaturas_individuales_retornar", parametros).FirstOrDefault();
        }

        #endregion Horas Vistas

        #region Personalizaciones Fernando Sor

        public IList<AdministrativoAsignaturaViewModel> ObtenerAdministrativosAsignaturas(string filtro_texto, string asignatura)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "admasi_asi_codigo", asignatura }
            };

            return institucion.BaseDatos.ExecuteQuery<AdministrativoAsignaturaViewModel>("paz_fernando_sor_aca_administrativos_asignaturas_retornar", parametros).ToList();
        }

        public AdministrativoAsignaturaViewModel ObtenerDetalleAdministrativosAsignaturas(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "admasi_consecutivoP", consecutivo }
            };

            return institucion.BaseDatos.ExecuteQuery<AdministrativoAsignaturaViewModel>("paz_fernando_sor_aca_administrativos_asignaturas_detalle_retornar", parametros).FirstOrDefault();
        }

        public void IngresarAdministrativoAsignatura(string asignatura, string persona)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "admasi_per_codigo", persona },
                { "admasi_asi_codigo", asignatura }
            };

            institucion.BaseDatos.ExecuteNonQuery("paz_fernando_sor_aca_administrativos_asignaturas_ingresar", parametros);
        }

        public void EliminarAdministrativoAsignatura(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "admasi_consecutivoP", consecutivo }
            };

            institucion.BaseDatos.ExecuteNonQuery("paz_fernando_sor_aca_administrativos_asignaturas_eliminar", parametros);
        }

        #endregion Personalizaciones Fernando Sor

        #region Tipos Programas

        public IList<TiposProgramasViewModel> ObtenerTiposProgramas(bool? estado, string texto)
        {
            var parametros = new Dictionary<string, object>(){
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtros_estado", estado },
                { "filtros_texto", texto }
            };

            return institucion.BaseDatos.ExecuteQuery<TiposProgramasViewModel>("pa_aca_tipos_programas_retornar", parametros);
        }

        public TiposProgramasViewModel ObtenerDetalleTipoPrograma(string id)
        {
            var parametros = new Dictionary<string, object>(){
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "tippro_codigoP", id}
            };

            return institucion.BaseDatos.ExecuteQuery<TiposProgramasViewModel>("pa_aca_tipos_programas_detalle_retornar", parametros).FirstOrDefault();
        }

        public int IngresarTipoPrograma(string tipo_codigo, string nombre, bool estado, bool tlc, bool? aplica_examen, bool? aplica_doc, DateTime fecha_inicio, DateTime? fecha_limite, string mensaje_coreo = null)
        {
            var parametros = new Dictionary<string, object>(){
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "tippro_codigoP", tipo_codigo },
                { "tippro_nombre", nombre },
                { "tippro_estado", estado },
                { "tippro_aplica_examen", aplica_examen },
                { "tippro_aplica_documentos", aplica_doc },
                { "tippro_fecha_inicio_preinscripcion", fecha_inicio },
                { "tippro_fecha_limite_preinscripcion", fecha_limite },
                { "tippro_mensaje_correo", mensaje_coreo },
                { "tippro_tlc", tlc },
                { "aud_usuario_sesion", per_codigoP },
                { "aud_ip", ClientIpAddress },
                { "aud_cat_codigo", CategoriaAuditoria },
                { "aud_interno", false },
                { "aud_usuario_q10", codigo_ejecutivo_institucion }
            };

            return institucion.BaseDatos.ExecuteNonQuery("pa_aca_tipos_programas_ingresar", parametros);
        }

        public void ModificarTipoPrograma(string tipo_codigo, string nombre, bool estado, bool tlc, bool? aplica_examen, bool? aplica_doc, DateTime fecha_inicio, DateTime? fecha_limite)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "tippro_codigoP", tipo_codigo },
                { "tippro_nombre", nombre },
                { "tippro_estado", estado },
                { "tippro_aplica_examen", aplica_examen },
                { "tippro_aplica_documentos", aplica_doc },
                { "tippro_fecha_inicio_preinscripcion", fecha_inicio },
                { "tippro_fecha_limite_preinscripcion", fecha_limite },
                { "tippro_tlc", tlc },
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_tipos_programas_modificar", parametros);
        }

        public void ModificarMensajeCorreoTipoPrograma(string id, string mensajeCorreo)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "tippro_codigoP", id },
                { "tippro_mensaje_correo", mensajeCorreo }
            };

            institucion.BaseDatos.ExecuteNonQuery("paz_cafam_cet_aca_tipos_programas_mensaje_correo_modificar", parametros);
        }

        public void EliminarTipoPrograma(string id)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "tippro_codigoP", id },
                { "aud_usuario_sesion", per_codigoP },
                { "aud_ip", ClientIpAddress },
                { "aud_cat_codigo", CategoriaAuditoria },
                { "aud_interno", false },
                { "aud_usuario_q10", codigo_ejecutivo_institucion }
            };

            institucion.BaseDatos.ExecuteQuery<TiposProgramasViewModel>("pa_aca_tipos_programas_eliminar", parametros);
        }

        public IList<DocumentoTipoProgramaViewModel> ObtenerDocumentosTipoPrograma(string id)
        {
            var parametros = new Dictionary<string, object>(){
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "tippro_codigoP", id}
            };

            return institucion.BaseDatos.ExecuteQuery<DocumentoTipoProgramaViewModel>("paz_cafam_cet_aca_tipos_programas_categorias_retornar", parametros);
        }

        public DocumentoTipoProgramaViewModel ObtenerDetalleDocumentoTipoPrograma(int id)
        {
            var parametros = new Dictionary<string, object>(){
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "tipprocat_consecutivoP", id}
            };

            return institucion.BaseDatos.ExecuteQuery<DocumentoTipoProgramaViewModel>("paz_cafam_cet_aca_tipos_programas_categorias_detalle_retornar", parametros).FirstOrDefault();
        }

        public void IngresarDocumentoTipoPrograma(string tipoPrograma, int? categoriaDocumento)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "tipprocat_tippro_codigo", tipoPrograma },
                { "tipprocat_doccat_consecutivo", categoriaDocumento }
            };

            institucion.BaseDatos.ExecuteNonQuery("paz_cafam_cet_aca_tipos_programas_categorias_ingresar", parametros);
        }

        public void EliminarDocumentoTipoPrograma(int id)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "tipprocat_consecutivoP", id },
            };

            institucion.BaseDatos.ExecuteNonQuery("paz_cafam_cet_aca_tipos_programas_categorias_eliminar", parametros);
        }

        #endregion

        #region Ofertas Academicas

        public IList<OfertasAcademicasViewModel> ObtenerOfertasAcademicas(int? periodo, int? sedjornada, string programa, string asignatura, bool? estado)
        {
            var parametros = new Dictionary<string, object>(){
                {"aplent_codigoP", institucion.aplent_codigoP},
                {"per_consecutivoP", periodo},
                {"sedjor_consecutivoP", sedjornada},
                {"pro_codigoP", programa},
                {"asi_codigoP", asignatura},
                {"filtro_estado", estado},
            };
            return institucion.BaseDatos.ExecuteQuery<OfertasAcademicasViewModel>("paz_cafam_cet_ofertas_academicas_retornar", parametros);
        }

        public IList<OfertasAcademicasViewModel> ObtenerOfertasAcademicasApi(int? periodo, int? sedjornada, string programa, string asignatura, bool? estado)
        {
            var parametros = new Dictionary<string, object>(){
                {"aplent_codigoP", institucion.aplent_codigoP},
                {"per_consecutivoP", periodo},
                {"sedjor_consecutivoP", sedjornada},
                {"pro_codigoP", programa},
                {"asi_codigoP", asignatura},
                {"filtro_estado", estado},
            };
            return institucion.BaseDatos.ExecuteQuery<OfertasAcademicasViewModel>("paz_cafam_cet_ofertas_academicas_api_retornar", parametros);
        }

        public OfertasAcademicasViewModel ObtenerDetalleOfertaAcademica(int id)
        {
            var parametros = new Dictionary<string, object>(){
                {"aplent_codigoP", institucion.aplent_codigoP},
                {"ofeaca_consecutivoP",  id}
            };
            return institucion.BaseDatos.ExecuteQuery<OfertasAcademicasViewModel>("paz_cafam_cet_ofertas_academicas_detalle_retornar", parametros).FirstOrDefault();
        }

        public IList<OfertasAcademicasViewModel> IngresarOfertaAcademica(int periodo, string nombre, int sedjornada, string programa, string asignatura, decimal? intensidad, string nivel, int? edad_min, int? edad_max, DateTime fecha_inicio, int? cupo_maximo, bool estado)
        {
            var parametros = new Dictionary<string, object>(){
                {"aplent_codigoP", institucion.aplent_codigoP},
                {"ofeaca_nombre", nombre},
                {"ofeaca_per_consecutivo",  periodo},
                {"ofeaca_sedjor_consecutivo",  sedjornada},
                {"ofeaca_pro_codigo",  programa},
                {"ofeaca_asi_codigo",  asignatura},
                {"ofeaca_intensidad",  intensidad},
                {"ofeaca_niv_codigo",  nivel},
                {"ofeaca_edad_minima",  edad_min},
                {"ofeaca_edad_maxima",  edad_max},
                {"ofeaca_fecha_inicio_clases",  fecha_inicio},
                {"ofeaca_cupo_maximo",  cupo_maximo},
                {"ofeaca_estado",  estado},
            };
            return institucion.BaseDatos.ExecuteQuery<OfertasAcademicasViewModel>("paz_cafam_cet_ofertas_academicas_ingresar", parametros);
        }

        public IList<OfertasAcademicasViewModel> ModificarOfertaAcademica(int id, string nombre, decimal? intensidad, string asignatura, string nivel, int? edad_min, int? edad_max, DateTime fecha_inicio, int? cupo_maximo, bool estado)
        {
            var parametros = new Dictionary<string, object>()
            {
                {"aplent_codigoP", institucion.aplent_codigoP},
                {"ofeaca_consecutivoP", id},
                {"ofeaca_nombre", nombre},
                {"ofeaca_asi_codigo", asignatura},
                {"ofeaca_niv_codigo", nivel},
                {"ofeaca_intensidad",  intensidad},
                {"ofeaca_edad_minima",  edad_min},
                {"ofeaca_edad_maxima",  edad_max},
                {"ofeaca_fecha_inicio_clases",  fecha_inicio},
                {"ofeaca_cupo_maximo",  cupo_maximo},
                {"ofeaca_estado",  estado},
            };
            return institucion.BaseDatos.ExecuteQuery<OfertasAcademicasViewModel>("paz_cafam_cet_ofertas_academicas_modificar", parametros);
        }

        public void EliminarOfertaAcademica(int id)
        {
            var parametros = new Dictionary<string, object>()
            {
                {"aplent_codigoP", institucion.aplent_codigoP},
                {"ofeaca_consecutivoP",  id}
            };
            institucion.BaseDatos.ExecuteQuery<OfertasAcademicasViewModel>("paz_cafam_cet_ofertas_academicas_eliminar", parametros);
        }

        #endregion

        #region Escala Evaluación

        public IPagedList<EscalaEvaluacionViewModel> ObtenerEscalasEvaluacion(string texto, bool aplicaPaginacion, int pageIndex = 1, int pageSize = 12, string tipoPrograma = null)
        {
            int totalCount;
            var parametros = new Dictionary<string, object>(){
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtro_texto", texto },
                { "esceva_tippro_codigo", tipoPrograma },
                { "paginacion", aplicaPaginacion }
            };
            var result = institucion.BaseDatos.ExecuteQuery<EscalaEvaluacionViewModel>("pa_aca_escalas_evaluacion_retornar", parametros, pageIndex, pageSize, out totalCount);
            return new StaticPagedList<EscalaEvaluacionViewModel>(result, pageIndex, pageSize, totalCount);
        }

        public EscalaEvaluacionViewModel ObtenerDetalleEscalaEvaluacion(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "esceva_consecutivoP", consecutivo}
            };

            return institucion.BaseDatos.ExecuteQuery<EscalaEvaluacionViewModel>("pa_aca_escalas_evaluacion_detalle_retornar", parametros).FirstOrDefault();
        }

        public int IngresarEscalaEvaluacion(decimal? desde, decimal? hasta, char? letra, string significado, int? puntos, string abreviacion, decimal? puntos2, string codigoTipoPrograma = null)
        {
            var parametros = new Dictionary<string, object>(){
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "esceva_desde", desde },
                { "esceva_hasta", hasta },
                { "esceva_letra", letra },
                { "esceva_significado", significado },
                { "esceva_puntos", puntos },
                { "esceva_abreviacion", abreviacion },
                { "esceva_puntos2", puntos2 },
                { "esceva_tippro_codigo", codigoTipoPrograma },
            };

            return institucion.BaseDatos.ExecuteNonQuery("pa_aca_escalas_evaluacion_ingresar", parametros);
        }

        public void ModificarEscalaEvaluacion(int consecutivo, decimal? desde, decimal? hasta, char? letra, string significado, int? puntos, string abreviacion, decimal? puntos2, string codigoTipoPrograma = null)
        {
            var parametros = new Dictionary<string, object>(){
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "esceva_consecutivoP", consecutivo },
                { "esceva_desde", desde },
                { "esceva_hasta", hasta },
                { "esceva_letra", letra },
                { "esceva_significado", significado },
                { "esceva_puntos", puntos },
                { "esceva_abreviacion", abreviacion },
                { "esceva_puntos2", puntos2 },
                { "esceva_tippro_codigo", codigoTipoPrograma }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_escalas_evaluacion_modificar", parametros);
        }

        public void EliminarEscalaEvaluacion(int consecutivo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "esceva_consecutivoP", consecutivo}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_escalas_evaluacion_eliminar", parametros);
        }

        #endregion Escala Nacionales    

        #region TemasAsignatura
        public IList<TemasViewModel> ObtenerTemasAsignaturas(string asiCodigo, string texto = null, char? estado = null)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "asitem_asi_codigo", asiCodigo },
                { "filtro_texto", texto },
                { "asitem_estado", estado }
            };

            return institucion.BaseDatos.ExecuteQuery<TemasViewModel>("pa_aca_asignaturas_temas_retornar", parametros).ToList();
        }

        public TemasViewModel ObtenerDetalleTemaAsignaturas(string codigoTema)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "asitem_codigoP", codigoTema }
            };

            return institucion.BaseDatos.ExecuteQuery<TemasViewModel>("pa_aca_asignaturas_temas_detalle_retornar", parametros).FirstOrDefault();
        }

        public void IngresarTemaAsignatura(string codigo, string asiCodigo, string nombre, int? intensidadHoraria, bool estado)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "asitem_asi_codigo", asiCodigo },
                { "asitem_codigoP", codigo },
                { "asitem_nombre", nombre },
                { "asitem_intensidad_horaria", intensidadHoraria },
                { "asitem_estado", estado }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_asignaturas_temas_ingresar", parametros);
        }

        public void ModificarTemaAsignatura(string codigo, string nombre, int? intensidadHoraria, bool estado)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "asitem_codigoP", codigo },
                { "asitem_nombre", nombre },
                { "asitem_intensidad_horaria", intensidadHoraria },
                { "asitem_estado", estado }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_asignaturas_temas_modificar", parametros);
        }

        public void EliminarTemaAsignatura(string asiCodigo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "asitem_codigoP", asiCodigo }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_asignaturas_temas_eliminar", parametros);
        }
        #endregion TemasAsignatura

        #region Competencias

        public IPagedList<CompetenciaViewModel> ObtenerCompetencias(bool? estado, string texto, int pagina = 1, int tamanioPagina = Constantes.PAGINACION_TAMANO_PAGINA)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtro_texto", texto },
                { "com_estado", estado }
            };

            var result = institucion.BaseDatos.ExecuteQuery<CompetenciaViewModel>("pa_aca_competencias_retornar", parametros, pagina, tamanioPagina, out int totalCount);
            return new StaticPagedList<CompetenciaViewModel>(result, pagina, tamanioPagina, totalCount);
        }

        public IList<AsignaturaCompetenciaViewModel> ObtenerCompetenciasAsignatura(string codigoAsignatura)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "asicom_asi_codigo", codigoAsignatura }
            };

            return institucion.BaseDatos.ExecuteQuery<AsignaturaCompetenciaViewModel>("pa_aca_asignaturas_competencias_retornar", parametros);
        }

        public AsignaturaCompetenciaViewModel ObtenerDetalleCompetenciasAsignatura(int consecutivoRelacion)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "asicom_consectivoP", consecutivoRelacion }
            };

            return institucion.BaseDatos.ExecuteQuery<AsignaturaCompetenciaViewModel>("pa_aca_asignaturas_competencias_detalle_retornar", parametros).FirstOrDefault();
        }

        public CompetenciaViewModel ObtenerDetalleCompetencia(string codigo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "com_codigoP", codigo }
            };

            return institucion.BaseDatos.ExecuteQuery<CompetenciaViewModel>("pa_aca_competencias_detalle_retornar", parametros).FirstOrDefault();
        }

        public void IngresarCompetencia(string codigo, string nombre, string abreviacion, bool estado)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "com_codigoP", codigo },
                { "com_nombre", nombre },
                { "com_abreviacion", abreviacion },
                { "com_estado", estado },
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_competencias_ingresar", parametros);
        }

        public void ModificarCompetencia(string codigo, string nombre, string abreviacion, bool estado)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "com_codigoP", codigo },
                { "com_nombre", nombre },
                { "com_abreviacion", abreviacion },
                { "com_estado", estado },
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_competencias_modificar", parametros);
        }

        public void EliminarCompetencia(string codigo)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "com_codigoP", codigo}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_competencias_eliminar", parametros);
        }

        public void IngresarRelacionAsignaturaCompetencia(string codigoAsignatura, string codigoCompetencia)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "asicom_asi_codigo", codigoAsignatura },
                { "asicom_com_codigo", codigoCompetencia }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_asignaturas_competencias_ingresar", parametros);
        }

        public void EliminarRelacionAsignaturaCompetencia(int consecutivoRelacion)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "asicom_consectivoP", consecutivoRelacion }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_asignaturas_competencias_eliminar", parametros);
        }

        #endregion Competencias

        #region Escalas valorativas 

        public IPagedList<EscalaValorativaViewModel> ObtenerEscalasValorativas(string texto, int? consecutivoPeriodo, int pagina = 1)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "filtro_texto", texto },
                { "escval_per_consecutivo", consecutivoPeriodo }
            };

            var result = institucion.BaseDatos.ExecuteQuery<EscalaValorativaViewModel>("pa_aca_escalas_valorativas_retornar", parametros, pagina, Constantes.PAGINACION_TAMANO_PAGINA, out int totalCount);
            return new StaticPagedList<EscalaValorativaViewModel>(result, pagina, Constantes.PAGINACION_TAMANO_PAGINA, totalCount);
        }

        public EscalaValorativaViewModel ObtenerDetalleEscalaValorativa(int consecutivoEscala)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "escval_consecutivoP", consecutivoEscala }
            };

            return institucion.BaseDatos.ExecuteQuery<EscalaValorativaViewModel>("pa_aca_escalas_valorativas_detalle_retornar", parametros).FirstOrDefault();
        }

        public void IngresarEscalaValorativa(string nombre, string abreviatura, int consecutivoPeriodo, bool condicion, int nota)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "escval_nombre", nombre },
                { "escval_abreviatura", abreviatura },
                { "escval_per_consecutivo", consecutivoPeriodo },
                { "escval_condicion", condicion },
                { "escval_nota_escala", nota }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_escalas_valorativas_ingresar", parametros);
        }

        public void ModificarEscalaValorativa(int consecutivoEscala, string nombre, string abreviatura, int consecutivoPeriodo, bool condicion, int nota)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "escval_consecutivoP", consecutivoEscala },
                { "escval_nombre", nombre },
                { "escval_abreviatura", abreviatura },
                { "escval_per_consecutivo", consecutivoPeriodo },
                { "escval_condicion", condicion },
                { "escval_nota_escala", nota }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_escalas_valorativas_modificar", parametros);
        }

        public void EliminarEscalaValorativa(int consecutivoEscala)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "escval_consecutivoP", consecutivoEscala}
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_escalas_valorativas_eliminar", parametros);
        }

        #region Detalles escala

        public IList<DetalleEscalaValorativaViewModel> ObtenerDetallesEscalaValorativa(int consecutivoEscala)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "escvaldet_escval_consecutivo", consecutivoEscala }
            };

            return institucion.BaseDatos.ExecuteQuery<DetalleEscalaValorativaViewModel>("pa_aca_escalas_valorativas_detalles_retornar", parametros);
        }

        public DetalleEscalaValorativaViewModel ObtenerDetalleDetallesEscalaValorativa(int consecutivoDetalle)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "escvaldet_consecutivoP", consecutivoDetalle }
            };

            return institucion.BaseDatos.ExecuteQuery<DetalleEscalaValorativaViewModel>("pa_aca_escalas_valorativas_detalles_detalle_retornar", parametros).FirstOrDefault();
        }

        public void IngresarDetalleEscalaValorativa(int consecutivoEscala, decimal rangoInicial, decimal rangoFinal, int calificacionVigesimal)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "escvaldet_escval_consecutivo", consecutivoEscala },
                { "escvaldet_rango_inicial", rangoInicial },
                { "escvaldet_rango_final", rangoFinal },
                { "escvaldet_calificacion_vigesimal", calificacionVigesimal }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_escalas_valorativas_detalles_ingresar", parametros);
        }

        public void ModificarDetalleEscalaValorativa(int consecutivoDetalle, decimal rangoInicial, decimal rangoFinal, int calificacionVigesimal)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "escvaldet_consecutivoP", consecutivoDetalle },
                { "escvaldet_rango_inicial", rangoInicial },
                { "escvaldet_rango_final", rangoFinal },
                { "escvaldet_calificacion_vigesimal", calificacionVigesimal }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_escalas_valorativas_detalles_modificar", parametros);
        }

        public void EliminarDetalleEscalaValorativa(int consecutivoDetalle)
        {
            var parametros = new Dictionary<string, object>() {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "escvaldet_consecutivoP", consecutivoDetalle }
            };

            institucion.BaseDatos.ExecuteNonQuery("pa_aca_escalas_valorativas_detalles_eliminar", parametros);
        }

        #endregion Detalles escala

        #endregion Escalas valorativas 

        #region Curso rotativo

        public async Task<PlanAcademicoViewModel> ObtenerDetalleCursoRotativo(int? consecutivoPlanAcademico)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaaca_consecutivoP", consecutivoPlanAcademico}
            };

            return (await institucion.BaseDatos.ExecuteQueryAsync<PlanAcademicoViewModel>("pa_aca_apis_planes_academicos_colegios_detalle_retornar", parametros)).FirstOrDefault();
        }

        public async Task<IList<PeriodoPorcentajeViewModel>> ObtenerPeriodoCursoRotativo(int? consecutivoPlanAcademico)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaacagraper_plaaca_consecutivo", consecutivoPlanAcademico}
            };

            return (await institucion.BaseDatos.ExecuteQueryAsync<PeriodoPorcentajeViewModel>("pa_aca_apis_planes_academicos_grados_periodos_retornar", parametros));
        }

        public async Task<IList<DocenteApoyoViewModel>> ObtenerDocenteApoyoCursoRotativo(int? consecutivoPlanAcademico)
        {
            var parametros = new Dictionary<string, object>()
            {
                { "aplent_codigoP", institucion.aplent_codigoP },
                { "plaaca_consecutivoP", consecutivoPlanAcademico}
            };

            return (await institucion.BaseDatos.ExecuteQueryAsync<DocenteApoyoViewModel>("pa_aca_apis_planes_academicos_docentes_retornar", parametros));
        }

        #endregion
    }
}

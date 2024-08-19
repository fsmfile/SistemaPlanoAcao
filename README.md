# Power apps em PT-BR e separador de fórmulas é ;;

# Endereço Git: https://raw.githubusercontent.com/fsmfile/SistemaPlanoAcao/main/README.md

# SistemaPlanoAcao

# Power Apps

[Dados]{

# Os Dados são listas Sharepoint
    
[tbl_cad_ResponsaveisONS_planoAcao]{
    [ID_PlanoAcao]: Consulta {ID: tbl_cad_PlanoAcao}
    [Usuario]: Consulta {LoginSSO: tbl_cad_UsuarioSistema}
}

[tbl_cad_entidades]{
    [NomeEntidade]: texto pk
    [Descricao]: texto
}

[tbl_cad_AtualizacoesRealizadasPlanoAcao]{
    [ID_PlanoAcao]: Consulta {ID: tbl_cad_PlanoAcao}
    [AtualizacaoRealizada]: texto com várias linhas
    [DtAtualizacao]: Data e hora (dd/mm/aaaa)
}

[tbl_cad_Status_planoacao]{
    [NomeStatus]: texto pk
    [Descricao]: texto
}

[tbl_cad_PlanoAcao]{
    [NomeAcao]: Consulta {NomeAcao: tbl_cad_NomeAcao}
    [DtInicio]: Data e hora (dd/mm/aaaa)
    [DtAlvo]: Data e hora (dd/mm/aaaa)
    [Descricao]: texto com várias linhas
    [NomeStatus]: Consulta {NomeStatus: tbl_cad_Status_planoacao}
}

[tbl_cad_GT]{
    [NomeGT]: texto pk
    [Descricao]: texto
}

[tbl_cad_NomeAcao]{
    [NomeAcao]: texto pk
    [NomeGT]: Consulta {NomeGT: tbl_cad_GT}
}

[tbl_cad_UsuarioSistema]{
    [matFuncionario]: Número inteiro
    [LoginSSO]: texto
    [Ativo_UsuarioSistema]: boolean
    [Grupo]: Consulta {Grupo_grupoUsuario: tbl_cad_GrupoSistema}
}

}


# Dicionário
Dicionário{
    "iniciais: Tipo"
    btn: Botão
    txt: Campo texto
    Selec: Seletor de Data
    Dt: Caixa de Texto
    Cbo: Caixa de Combinação
    Ctn: Conteiner
    Lst: Galeria vertical
    txtRichText: Texto Longo RichText
    Header: Tipo Cabeçalho
}

[App]{}



[TelaInicial]{


    [Ctn_FormCadPlanoAcao]{
        btnNovoFormPlanoAcao{
            // Limpa os campos
    Reset(CboNomeAcao);;
    Reset(SelecDtInicio_Acao);;
    SelecDtInicio_Acao.SelectedDate = Today();;
    Reset(SelecDtAlvo_Acao);;
    SelecDtAlvo_Acao.SelectedDate = Blank();;
    // Limpeza específica para os campos que não foram resetados
    txtRichTextDescricao_Acao.HtmlText = "";; // Ajuste para limpar o RichText
    Reset(CboStatusAcao);;

// Exibe uma notificação informando que o formulário está pronto para novo cadastro
Notify("Formulário pronto para novo cadastro!"; NotificationType.Information)

        }
        btnSalvarFormPlanoAcao{
            OnSelect{
                If(
    IsBlank(CboNomeAcao.Selected.NomeAcao) || IsBlank(txtRichTextDescricao_Acao.HtmlText);
    Notify("Preencha todos os campos obrigatórios antes de salvar!"; NotificationType.Error);
    Patch(
        tbl_cad_PlanoAcao;
        Defaults(tbl_cad_PlanoAcao);
        {
            DtInicio: SelecDtInicio_Acao.SelectedDate;
            DtAlvo: SelecDtAlvo_Acao.SelectedDate;
            NomeAcao: {
                '@odata.type': "#Microsoft.Azure.Connectors.SharePoint.SPListExpandedReference";
                Id: LookUp(tbl_cad_NomeAcao; NomeAcao = CboNomeAcao.Selected.NomeAcao).ID;
                Value: CboNomeAcao.Selected.NomeAcao
            };
            NomeStatus: {
                '@odata.type': "#Microsoft.Azure.Connectors.SharePoint.SPListExpandedReference";
                Id: LookUp(tbl_cad_Status_planoacao; NomeStatus = CboStatusAcao.Selected.NomeStatus).ID;
                Value: CboStatusAcao.Selected.NomeStatus
            };
            Descricao: txtRichTextDescricao_Acao.HtmlText
        }
    );;
    Notify("Plano de Ação salvo com sucesso!"; NotificationType.Success)
    
)

            }
        }

        [txtRichTextDescricao_Acao]{
            Referente ao campo: Descricao tbl_cad_PlanoAcao
            Default{
                Blank()
            }
        }

        [SelecDtAlvo_Acao]{
            Referente ao campo: DtAlvo tbl_cad_PlanoAcao
            DefaultDate{
                Blank()
            }
        }

        [SelecDtInicio_Acao]{
            Referente ao campo: DtInicio tbl_cad_PlanoAcao
            DefaultDate{
                Today()
            }
        }

        [CboStatusAcao]{
            Referente ao campo: NomeStatus tbl_cad_Status_planoacao
            DisplayFields{
                ["NomeStatus"]
            }
            Items{
                tbl_cad_Status_planoacao
            }
            SelectMultiple{
                false
            }
        }

        [CboNomeAcao]{
            Referente ao campo: NomeAcao tbl_cad_PlanoAcao
            DisplayFields{
                ["NomeAcao"]
            }
            Items{
                tbl_cad_NomeAcao
            }
            SelectMultiple{
                false
            }
        }

    }


    Ctn_LstPlanosAcao{
        txtLocalizaPlanoAcao{
            Default{
                Blank()
            }
        }
        LstPlanosAcao{
            Campos{

                txtNomeAcao_lstPlanosAcao{
                ThisItem.NomeAcao.Value & "          ::          " & Text(ThisItem.DtInicio; "[$-en-US]dd/mm/yyyy") & "          ::          " & Text(ThisItem.DtAlvo; "[$-en-US]dd/mm/yyyy") & "          ::          " & ThisItem.NomeStatus.Value & "          ::          " & LookUp(tbl_cad_NomeAcao; NomeAcao = ThisItem.NomeAcao.Value;NomeGT.Value)
            }

            }
        
        Items{
            tbl_cad_PlanoAcao
        }
            

        }

    }

    Ctn_TabelasVinculadas{
        Ctn_ResponsaveisONS{
            btnSalvarRespONSAcao{}
            CboNomeResponsavelONS{}
            LstResponsaveisONS_Acao{
                btnExcluirResponsaveisONS{}
                txtResponsavelONS{}
        
            }
        }

        Ctn_Responsaveis{
            btnSalvarResponsaveis{}
            CboNomeResponsavel{}
            LstResponsaveis_Acao{
                btnExcluirResponsavel_Acao{}
                txtResponsavelAcao{}

            }
        }

        CtnAcompanhamentoAcao{
            btnNovoAcompAcao{}
            btnSalvarAcompAcao{}
            txtRichTextAtualizacaoAcao{}
            LstAcompanhamentosAcao{
                BtnExcluirAcompanhamentoAcao{}
                txtDtAtualizacaoAcomp{}
                txtDescricaoAcompAcao{}

            }
        }
    }

    CtnHeader{
        Header{
            logo: 'MarcasONS_Secundarias_verticais_Branca (1)'
            Title: "Sistema de Planos de Ação"
            LogoTooltip: "Sistema de Controle de Planos de Ação"
        }
    }

}
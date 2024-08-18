# SistemaPlanoAcao

# Power Apps

[Tela Inicial]{


# Dicionário{
    iniciais: Tipo
    btn: Botão
    txt: Campo texto
    Selec: Seletor de Data
    Dt: Caixa de Texto
    Cbo: Caixa de Combinação
    Ctn: Conteiner
    Lst: Galeria
    txtRichText: Texto Longo RichText
    Header: Tipo Cabeçalho
}


    Ctn_FormCadPlanoAcao{
        btnNovoFormPlanoAcao{}
        btnNovoFormPlanoAcao{}
        btnSalvarFormPlanoAcao{}
        txtRichTextDescricao_Acao{}
        SelecDtAlvo_Acao{}
        SelecDtInicio_Acao{}
        CboStatusAcao{}
        CboNomeAcao{}

    }

    Ctn_LstPlanosAcao{
        txtLocalizaPlanoAcao{}
        LstPlanosAcao{
            StatusAcao_lstPlanosAcao (caixa de texto){}
            DtAlvo_lstPlanosAcao{}
            DtInicio_lstPlanosAcao{}
            txtNomeGT_lstPlanosAcao{}
            txtNomeAcao_lstPlanosAcao{}

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
        Header
    }



}
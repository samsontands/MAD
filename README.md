# MAD

option extendobscounter = no;
options macrogen symbolgen mlogic mprint mfile;
options compress=yes;



%let version		= v10;
/*%let versionHitRate	= v6;*/
/*** remember defined sampleBase ***/
*AOA;
/*%let sampleBase = AOA;*/
/*%let sampleBase2 = AOA;*/
/*%let sampleBase3 = AOA;*/
*nonAOA;
%let sampleBase = Non_AOA;
%let sampleBase2 = Non-AOA;
%let sampleBase3 = NonAOA;
*nonAOA9798;
/*%let sampleBase = Non_AOA;*/
/*%let sampleBase2 = Non-AOA;*/
/*%let sampleBase3 = NonAOA9798;*/


%include 		"C:\Users\115861\Desktop\MGA\06. program\07. development\000. macro_MAD.sas";
%include 		"C:\Users\115861\Desktop\MGA\06. program\07. development\000. macro_MAD9798.sas";
%include 		"C:\Users\115861\Desktop\MGA\06. program\07. development\000. macro_MAD98.sas";
%include 		"C:\Users\115861\Desktop\MGA\06. program\07. development\000. macro_GINI.sas";
libname outt 	"D:\TL2\17. MG_ascore\01. Data";
/*libname temp 	"C:\Users\115861\Desktop\mga_temp\&sampleBase3.2\";*/
libname temp 	"C:\Users\115861\Desktop\mga_temp\MAD_Gini_summ\";
libname temp	"C:\Users\115861\Desktop\mga_temp\nA_singleDimension\";
libname temp1 	"C:\Users\115861\Desktop\mga_temp\";
libname tt "C:\Users\115861\Desktop";






*remember change base here;
data bin; set /*outt.bin_v4_additional*/ outt.bin_v6;
	where f_aoa_fnl2="&sampleBase2";
run; /*37,983 - 40,944*/ /*38,489 - 42,082*/ /*36,896 - 40,999*/ /*40995(exclude exempted)*/


/*factor listing*/
proc sql; create table list as
	select distinct fieldnm, var, cats('d',var) as dvar
	from bin
	order by fieldnm;
quit; /*3,163*/ /*3,355*/ /*3,370*/

/*defined global variable*/
proc sql noprint;
	select count(dvar) into: dscnt
	from list; 
 
	%let dscnt = %trim(&dscnt);
	select dvar into: dsname1-: dsname&dscnt
	from list;   
quit; 
%put &dsname5;
proc sql noprint; 
	select sum(good) into:total_good  
	from bin (where=(fieldnm="S1")); 
quit;
proc sql noprint; 
	select sum(bad) into:total_bad  
	from bin (where=(fieldnm="S1")); 
quit;
%put &dscnt.	&total_good.	&total_bad.;	
/*AOA	 : 3,163 -- 1,905 -- 301*/ /*AOA	 : 3,370 -- 2,230 -- 196*/
/*Non-AOA: 3,163 -- 6,367 -- 150*/ /*Non-AOA : 3,370 -- 8,101 -- 179*/ /*Non-AOA(excluded exempted : 3,370 -- 8,045 -- 173*/







proc sql; create table temp1.data_bin_full_info2_v6 as 
/*proc sql; create table temp1.data_bin_full_info2_v5 as */
/*proc sql; create table temp1.data_bin_full_info2_v4_addit2 as */
/*proc sql; create table temp1.data_bin_full_info2_v4_additi as */
/*proc sql; create table temp1.data_bin_full_info2_v4 as */
	select t1.*, t2.f_aoa_fnl2, t2.T_60_conpl_dA_18m_gp, t2.Bad, t2.Ind, t2.Good, t2.CCris, t2.sample,
			t2.glos_ref_no, t2.glos_ref_no_2, t2.glos_app_id, t2.glos_app_cif_id, t2.ccris_key, t2.key, 
          	t2.cb_Max_Delinquency_L12M_All, t2.ID_NUMBER, t2.cif_no, t2.glos_ref_no_t2, 
          	t2.app_creation_dt, t2.app_creation_mth, t2.app_creation_mth2, 
          	t2.glos_creation_mth, t2.glos_creation_mth2, t2.yearCreation, t2.yearCreation2, 
          	t2.decision_mth, t2.decision_mth2, t2.yearDecision, t2.yearDecision2, 
          	t2.earliest_perf_mth, t2.earliest_perf_mth2, t2.yearMOB, t2.yearMOB2, 
          	t2.f_aoa_fnl, t2.f_aoa_1, t2.f_aoa_2, t2.f_OPP, t2.f_CPF, 
          	t2.policy1_fail_dsr_AOA, t2.policy2_subsales_OD_nonAOA, t2.policy3_SGD3k, t2.policy4_Inc3k, t2.policy5_creditHunger_AOA, 
          	t2.prop300kInc5k_AOA, t2.JohorRegion, t2.Johor_1617, t2.southernRegion, 
          	t2.f_fail_dsr, 
          	t2.rules, t2.waterfall2, 
          	t2.decision, 
          	t2.f_perf_period_18m, t2.T_60_conpl_dA_18m, t2.ind_30_conpl_dA_18m, t2.T_60_conpl_18m, t2.ind_30_conpl_18m, 
          	t2.SingleBorrwerApp, 
          	t2.T_60_conpl_impD_A_18m, t2.ind_30_conpl_impD_A_18m, 
          	t2.in_time, t2.waterfall3,  
          	t2.ever_drp_18m, t2.ever_akpk_18m
/*	select t1.*, t2.f_aoa_fnl2, t2.T_60_conpl_dA_18m_gp, t2.Bad, t2.Ind, t2.Good, t2.CCris, t2.sample, t2.sample2*/
	from outt.data_bin_full_dev t1 left join ccris_factors_base2 t2 on (t1.refno_cifID_key=t2.refno_cifID_key); 
/*	from outt.data_bin_full9392 t1 left join ccris_factors_base2 t2 on (t1.refno_cifID_key=t2.refno_cifID_key); */
/*	from outt.data_bin_additional2 t1 left join ccris_factors_base t2 on (t1.refno_cifID_key=t2.refno_cifID_key); */
/*	from outt.data_bin_additional t1 left join ccris_factors_base t2 on (t1.refno_cifID_key=t2.refno_cifID_key); */
/*	from outt.data_bin_full t1 left join ccris_factors_base t2 on (t1.refno_cifID_key=t2.refno_cifID_key);*/
quit; /*210,552*/
/************************** extra added - to exclude exempted segment **************************/
/************************** extra added - to exclude exempted segment **************************/
proc sql; create table MG_segment as
	select glos_ref_no, loan_segment_cd, app_category_code
	from mga_2021.ttd_l2_tx_202108
	where product = 'MG'
	order by glos_ref_no;
quit; /*222,*/
proc sql; create table temp1.data_bin_full_info2_v6 as
	select t1.*, t2.glos_ref_no as glos_ref_no_222, t2.loan_segment_cd, t2.app_category_code
	from temp1.data_bin_full_info2_v6 t1 left join MG_segment t2 on (t1.glos_ref_no=t2.glos_ref_no);
quit; /*210,552*/
/************************** extra added - to exclude exempted segment **************************/
/************************** extra added - to exclude exempted segment **************************/
*base;
data t_pre; set temp1.data_bin_full_info2_v6;
/*data t_pre; set temp1.data_bin_full_info2_v5;*/
/*data t_pre; set temp1.data_bin_full_info2_v4_addit2;*/
/*data t_pre; set temp1.data_bin_full_info2_v4_additi;*/
/*data t_pre; set temp1.data_bin_full_info2_v4;*/
	where sample='in_time_development' and T_60_conpl_dA_18m_gp ne 'Ind' 
		  and f_aoa_fnl2="&sampleBase2"
		  and loan_segment_cd in ('001','005','006','012','015','019','020','023','024','025') 
		  and app_category_code in ('HL01_HNW','HL02_Normal','HL03_DegreeGraduate','');

	if T_60_conpl_dA_18m_gp='Bad' 		then class=1;
	else if T_60_conpl_dA_18m_gp='Good' then class=0;

	array noHit 	dO_A_mth_snc_2MIAp		dO_A_mth_snc_2MIAp_2	dO_cc_mth_snc_2MIAp		dO_cc_mth_snc_2MIAp_2
					dO_cl_mth_snc_2MIAp		dO_cl_mth_snc_2MIAp_2	dO_hl_mth_snc_2MIAp		dO_hl_mth_snc_2MIAp_2
					dO_hp_mth_snc_2MIAp		dO_hp_mth_snc_2MIAp_2	dO_od_mth_snc_2MIAp		dO_od_mth_snc_2MIAp_2
					dO_pl_mth_snc_2MIAp		dO_pl_mth_snc_2MIAp_2	dO_rv_mth_snc_2MIAp		dO_rv_mth_snc_2MIAp_2
					dO_sc_mth_snc_2MIAp		dO_sc_mth_snc_2MIAp_2	dO_us_mth_snc_2MIAp		dO_us_mth_snc_2MIAp_2
					dO_A_mth_snc_3MIAp		dO_A_mth_snc_3MIAp_2	dO_cc_mth_snc_3MIAp		dO_cc_mth_snc_3MIAp_2
					dO_cl_mth_snc_3MIAp		dO_cl_mth_snc_3MIAp_2	dO_hl_mth_snc_3MIAp		dO_hl_mth_snc_3MIAp_2
					dO_hp_mth_snc_3MIAp		dO_hp_mth_snc_3MIAp_2	dO_od_mth_snc_3MIAp		dO_od_mth_snc_3MIAp_2
					dO_pl_mth_snc_3MIAp		dO_pl_mth_snc_3MIAp_2	dO_rv_mth_snc_3MIAp		dO_rv_mth_snc_3MIAp_2
					dO_sc_mth_snc_3MIAp		dO_sc_mth_snc_3MIAp_2	dO_us_mth_snc_3MIAp		dO_us_mth_snc_3MIAp_2;
	array lastMthCurr	dO_A_delBAL_ovr_A_ttlBAL_2 dO_A_DEL_Bal_1MIAp_2;

	do over noHit;
		if noHit ='34. 0' 			then noHit='99. MIA1';
	end;
/*	do over lastMthCurr;*/
/*		if lastMthCurr = '22. =0'	then lastMthCurr='99. MIA0';*/
/*	end;*/
run; /*2,206 -- 6,507*/ /*2,426 -- 8,280*/ /*8218(excluded exempted)*/


/*** for customize run MAD ***/ /*** for customize run MAD ***/ /*** for customize run MAD ***/
data t_pre; set t_pre;
	/*AOA*/
/*	if dO_A_perc_max_2MIAp_12m_2 ='05. -99995'		then dO_A_perc_max_2MIAp_12m_2 ='22. =0';*/
/*	if dO_A_perc_max_2MIAp_12m_2 ='07. -99993'		then dO_A_perc_max_2MIAp_12m_2 ='22. =0';*/
/**/
/*	if dO_A_cnt_evr_2MIAp_rt_12m_2 ='05. -99995'	then dO_A_cnt_evr_2MIAp_rt_12m_2 ='22. =0';*/
/*	if dO_A_cnt_evr_2MIAp_rt_12m_2 ='07. -99993'	then dO_A_cnt_evr_2MIAp_rt_12m_2 ='22. =0';*/
/**/
/*	if dO_A_delBAL_ovr_A_ttlBAL_2 ='05. -99995'		then dO_A_delBAL_ovr_A_ttlBAL_2 ='22. =0';*/
/*	if dO_A_delBAL_ovr_A_ttlBAL_2 ='07. -99993'		then dO_A_delBAL_ovr_A_ttlBAL_2 ='22. =0';*/
/*	if dO_A_delBAL_ovr_A_ttlBAL_2 ='04. -99996'		then dO_A_delBAL_ovr_A_ttlBAL_2 ='22. =0';*/
/**/
/*	if dO_A_DEL_Bal_1MIAp_2 ='05. -99995'			then dO_A_DEL_Bal_1MIAp_2 ='22. =0';*/
/*	if dO_A_DEL_Bal_1MIAp_2 ='07. -99993'			then dO_A_DEL_Bal_1MIAp_2 ='22. =0';*/
/**/
/*	if dO_A_mth_snc_2MIAp_2 ='05. -99995'			then dO_A_mth_snc_2MIAp_2 ='07. -99993';*/
/*	if dO_A_mth_snc_2MIAp_2 ='99. MIA1'				then dO_A_mth_snc_2MIAp_2 ='07. -99993';*/
/**/
/*	if dO_A_decrease_12m_2 ='05. -99995'			then dO_A_decrease_12m_2 ='34. 0';*/
/*	if dO_A_decrease_12m_2 ='07. -99993'			then dO_A_decrease_12m_2 ='34. 0';*/
/**/
/*	if dO_cl_ttlBAL_ovr_ttlBAL ='03. -99997'		then dO_cl_ttlBAL_ovr_ttlBAL ='22. =0';*/
/**/
/*	if dO_us_ovr_sc_balance ='04. -99996'			then dO_us_ovr_sc_balanceGp ='23. >0 - 2.5%';*/
/*	else if dO_us_ovr_sc_balance ='06. -99994'		then dO_us_ovr_sc_balanceGp ='44. >100%';*/
/*	else 												 dO_us_ovr_sc_balanceGp =dO_us_ovr_sc_balance;*/
/**/
/*	if dO_us_ovr_sc_balance ='04. -99996'			then dO_us_ovr_sc_balance ='23. >0 - 2.5%';*/
/*	if dO_us_ovr_sc_balance ='06. -99994'			then dO_us_ovr_sc_balance ='44. >100%';*/
/*	if dO_us_ovr_sc_balance ='05. -99995'			then dO_us_ovr_sc_balance ='44. >100%';*/
/*	if dO_us_ovr_sc_balance ='03. -99997'			then dO_us_ovr_sc_balance ='23. >0 - 2.5%';*/
/**/
/*	if dO_cl_perc_ovr_cl_lmt_12m ='03. -99997'		then dO_cl_perc_ovr_cl_lmt_12m ='22. =0';*/
/**/
/*	if dO_cl_perc_ovr_ttl_lmt_12m ='03. -99997'		then dO_cl_perc_ovr_ttl_lmt_12m ='22. =0';*/
/**/
/*	if dO_cl_perc_ovr_ttl_cnt_12m ='03. -99997'		then dO_cl_perc_ovr_ttl_cnt_12m ='22. =0';*/
/*	*/
/*	if dO_cl_perc_ovr_cl_cnt_12m ='03. -99997'		then dO_cl_perc_ovr_cl_cnt_12m ='22. =0';*/
/**/
/*	if dO_sc_Coll_Property ='34. 0'					then dO_sc_Coll_Property ='07. -99993';*/
/**/
/*	if dO_sc_Coll_Property ='03. -99997'			then dO_sc_Coll_PropertyGp ='07. -99993';*/
/*	else 												 dO_sc_Coll_PropertyGp =dO_sc_Coll_Property;*/

	/*non-AOA (not yet exclude exempted segment)*/
/*	if dO_sc_cnt_wrst_2MIAp_12m_2='07. -99993'		then dO_sc_cnt_wrst_2MIAp_12m_2 ='34. 0';*/
/**/
/*	if dO_A_perc_1MIAp_12m_2='07. -99993'			then dO_A_perc_1MIAp_12m_2 ='22. =0';*/
/*	if dO_A_perc_wrst_1MIAp_12m_2='07. -99993'		then dO_A_perc_wrst_1MIAp_12m_2 ='22. =0';*/
/**/
/*	if dO_sc_cnt_evr_1MIAp_rt_12m_2='07. -99993'	then dO_sc_cnt_evr_1MIAp_rt_12m_2 ='22. =0';*/
/*	if dO_sc_cnt_evr_2MIAp_rt_12m_2='07. -99993'	then dO_sc_cnt_evr_2MIAp_rt_12m_2 ='22. =0';*/
/*	if dO_A_cnt_evr_1MIAp_rt_12m_2='07. -99993'		then dO_A_cnt_evr_1MIAp_rt_12m_2 ='22. =0';*/
/**/
/*	if dO_A_delBAL_ovr_A_ttlBAL_2='07. -99993'		then dO_A_delBAL_ovr_A_ttlBAL_2 ='22. =0';*/
/*	if dO_A_DEL_Bal_1MIAp_2='07. -99993'			then dO_A_DEL_Bal_1MIAp_2 ='22. =0';*/
/**/
/*	if dO_sc_mth_snc_2MIAp_2='99. MIA1'				then dO_sc_mth_snc_2MIAp_2 ='07. -99993';*/
/*	if dO_A_mth_snc_2MIAp_2='99. MIA1'				then dO_A_mth_snc_2MIAp_2 ='07. -99993';*/
/**/
/*	if dO_A_decrease_12m_2 ='05. -99995'			then dO_A_decrease_12m_2 ='34. 0';*/
/*	if dO_A_decrease_12m_2 ='07. -99993'			then dO_A_decrease_12m_2 ='34. 0';*/
/**/
/*	if dO_us_ovr_sc_balance ='04. -99996'			then dO_us_ovr_sc_balanceGp ='23. >0 - 2.5%';*/
/*	else if dO_us_ovr_sc_balance ='06. -99994'		then dO_us_ovr_sc_balanceGp ='44. >100%';*/
/*	else 												 dO_us_ovr_sc_balanceGp =dO_us_ovr_sc_balance;*/

	/*non-AOA*/
	if dO_A_perc_wrst_1MIAp_12m_2='07. -99993'		then dO_A_perc_wrst_1MIAp_12m_2 ='22. =0';
	if dO_A_perc_1MIAp_12m_2='07. -99993'			then dO_A_perc_1MIAp_12m_2 ='22. =0';
	if dO_sc_cnt_wrst_2MIAp_12m_2='07. -99993'		then dO_sc_cnt_wrst_2MIAp_12m_2 ='34. 0';
	if dO_A_cnt_evr_1MIAp_rt_12m_2='07. -99993'		then dO_A_cnt_evr_1MIAp_rt_12m_2 ='22. =0';
	if dO_A_delBAL_ovr_A_ttlBAL_2='07. -99993'		then dO_A_delBAL_ovr_A_ttlBAL_2 ='22. =0';
	if dO_A_DEL_Bal_1MIAp_2='07. -99993'			then dO_A_DEL_Bal_1MIAp_2 ='22. =0';
	if dO_sc_mth_snc_2MIAp_2='99. MIA1'				then dO_sc_mth_snc_2MIAp_2 ='07. -99993';
	if dO_A_mth_snc_2MIAp_2='99. MIA1'				then dO_A_mth_snc_2MIAp_2 ='07. -99993';
	if dO_A_decrease_12m_2 ='05. -99995'			then dO_A_decrease_12m_2 ='34. 0';
	if dO_A_decrease_12m_2 ='07. -99993'			then dO_A_decrease_12m_2 ='34. 0';
	if dO_us_ovr_sc_balance ='04. -99996'			then dO_us_ovr_sc_balanceGp ='23. >0 - 2.5%';
	else if dO_us_ovr_sc_balance ='06. -99994'		then dO_us_ovr_sc_balanceGp ='44. >100%';
	else 												 dO_us_ovr_sc_balanceGp =dO_us_ovr_sc_balance;
run;
%mad(dO_A_perc_max_2MIAp_12m_2 		, 0.05, Down);
%mad(dO_A_cnt_evr_2MIAp_rt_12m_2 	, 0.05, Down);
%mad(dO_A_delBAL_ovr_A_ttlBAL_2 	, 0.05, Down);
%mad(dO_A_DEL_Bal_1MIAp_2 			, 0.05, Down);
%mad(dO_A_mth_snc_2MIAp_2 			, 0.05, Up);
%mad(dO_A_decrease_12m_2 			, 0.05, Down);
%mad(dO_cl_ttlBAL_ovr_ttlBAL		, 0.05, Down);
%mad(dO_us_ovr_sc_balance			, 0.05, Down);
%mad(dO_us_ovr_sc_balanceGp			, 0.05, Down);
%mad(dO_cl_perc_ovr_cl_lmt_12m		, 0.05, Down);
%mad(dO_cl_perc_ovr_ttl_lmt_12m		, 0.05, Down);
%mad(dO_cl_perc_ovr_ttl_cnt_12m		, 0.05, Down);
%mad(dO_cl_perc_ovr_cl_cnt_12m		, 0.05, Down);
%mad(dO_sc_Coll_Property			, 0.05, Up);
%mad(dO_sc_Coll_PropertyGp			, 0.05, Up);
/*non-AOA (not yet exclude exempted segment)*/
%mad(dO_sc_cnt_wrst_2MIAp_12m_2 	, 0.05, Down);
%mad(dO_A_perc_1MIAp_12m_2 			, 0.05, Down);		%mad98(dO_A_perc_1MIAp_12m_2 			, 0.05, Down);
%mad(dO_A_perc_wrst_1MIAp_12m_2		, 0.05, Down);		%mad98(dO_A_perc_wrst_1MIAp_12m_2		, 0.05, Down);
%mad(dO_sc_cnt_evr_1MIAp_rt_12m_2	, 0.05, Down);
%mad(dO_sc_cnt_evr_2MIAp_rt_12m_2	, 0.05, Down);		%mad9798(dO_sc_cnt_evr_2MIAp_rt_12m_2	, 0.05, Down);
%mad(dO_A_cnt_evr_1MIAp_rt_12m_2	, 0.05, Down);		%mad98(dO_A_cnt_evr_1MIAp_rt_12m_2		, 0.05, Down);
%mad(dO_A_delBAL_ovr_A_ttlBAL_2		, 0.05, Down);		%mad98(dO_A_delBAL_ovr_A_ttlBAL_2		, 0.05, Down);
%mad(dO_A_DEL_Bal_1MIAp_2			, 0.05, Down);		%mad98(dO_A_DEL_Bal_1MIAp_2				, 0.05, Down);
%mad(dO_sc_mth_snc_2MIAp_2			, 0.05, Up);
%mad(dO_A_mth_snc_2MIAp_2			, 0.05, Up);
%mad9798(dO_sc_mth_snc_1MIAp_2		, 0.05, Up);
%mad9798(dO_sc_sum_del_rt_12m_2		, 0.05, Down);
%mad(dO_A_decrease_12m_2			, 0.05, Down);		%mad98(dO_A_decrease_12m_2				, 0.05, Down);
%mad9798(dO_cc_utilisation			, 0.05, Down);
%mad9798(dO_cc_avg_utilisation		, 0.05, Down);
%mad(dO_us_ovr_sc_balanceGp			, 0.05, Down);
%mad9798(dO_cc_perc_ovr_ttl_lmt_12m	, 0.05, Down);		%mad9798(dO_cc_perc_ovr_ttl_lmt_12m		, 0.051, Down);
%mad98(dO_A_max_bureau_vintage		, 0.05, Up);
/*non-AOA*/
%mad(dO_A_perc_wrst_1MIAp_12m_2		, 0.05, Down);		%mad98(dO_A_perc_wrst_1MIAp_12m_2		, 0.05, Down);
%mad(dO_A_perc_1MIAp_12m_2 			, 0.05, Down);		%mad98(dO_A_perc_1MIAp_12m_2 			, 0.05, Down);
%mad(dO_sc_cnt_wrst_2MIAp_12m_2 	, 0.05, Down);
%mad(dO_A_cnt_evr_1MIAp_rt_12m_2	, 0.05, Down);		%mad98(dO_A_cnt_evr_1MIAp_rt_12m_2		, 0.05, Down);
%mad(dO_A_delBAL_ovr_A_ttlBAL_2		, 0.05, Down);		%mad98(dO_A_delBAL_ovr_A_ttlBAL_2		, 0.05, Down);
%mad(dO_A_DEL_Bal_1MIAp_2			, 0.05, Down);		%mad98(dO_A_DEL_Bal_1MIAp_2				, 0.05, Down);
%mad(dO_sc_mth_snc_2MIAp_2			, 0.05, Up);
%mad(dO_A_mth_snc_2MIAp_2			, 0.05, Up);
%mad9798(dO_sc_mth_snc_1MIAp_2		, 0.05, Up);
%mad9798(dO_sc_worst_del_12m_2		, 0.05, Down);
%mad(dO_A_decrease_12m_2			, 0.05, Down);		%mad98(dO_A_decrease_12m_2				, 0.05, Down);
%mad9798(dO_cc_utilisation			, 0.05, Down);
%mad9798(dO_cc_avg_utilisation		, 0.05, Down);
%mad(dO_us_ovr_sc_balanceGp			, 0.05, Down);
%mad9798(dO_cc_perc_ovr_ttl_lmt_12m	, 0.05, Down);		%mad9798(dO_cc_perc_ovr_ttl_lmt_12m		, 0.051, Down);
%mad9798(dO_cc_perc_ovr_ttl_cnt_12m	, 0.05, Down);
%mad98(dO_A_max_bureau_vintage		, 0.05, Up);
/*** for customize run MAD ***/ /*** for customize run MAD ***/ /*** for customize run MAD ***/



*MAD binning;
%macro RunMAD;
%do i = 1 %to &dscnt.;
	%put &&dsname&i;
	%mad(&&dsname&i	, 0.05, Down);
%end;
data down_appd; length varname $50; set temp.ap_:; run; 
data down_intm; length varname $50; set temp.in_:; run;
data down_fnl;  length varname $50; set temp.fn_:; run; 

%do i = 1 %to &dscnt.;
	%put &&dsname&i;
	%mad(&&dsname&i	, 0.05, Up);
%end;
data up_appd; length varname $50; set temp.ap_:; run;
data up_intm; length varname $50; set temp.in_:; run;
data up_fnl;  length varname $50; set temp.fn_:; run;
%mend;
%RunMAD;




*sort downtrend binning;
proc sql; create table temp.down_appd_&sampleBase3. as
	select t1.*, t2.fieldnm
	from down_appd t1 left join list t2 on (t1.varname=t2.dvar)
	order by t2.fieldnm, t1.round, t1.row, t1.ub;
quit; /*160,202*/ /*192,903*/ /*145,317*/ /*157,032*/ /*185,154*/ /*185,341(nonAOA exclude exempted)*/
proc sql; create table temp.down_intm_&sampleBase3. as
	select t1.*, t2.fieldnm
	from down_intm t1 left join list t2 on (t1.varname=t2.dvar)
	order by t2.fieldnm, t1.row, t1.ub;
quit; /*26,291*/ /*26,133*/ /*26,133*/ /*27,380*/ /*28,169*/ /*28,165(nonAOA exclude exempted)*/
proc sql; create table temp.down_fnl_&sampleBase3. as
	select t1.*, t2.fieldnm
	from down_fnl t1 left join list t2 on (t1.varname=t2.dvar)
	order by t2.fieldnm, t1.row, t1.ub;
quit; /*37,983*/ /*40,944*/ /*35,118*/ /*36,896*/ /*40,999*/ /*40,995(nonAOA exclude exempted)*/


*sort uptrend binning;
proc sql; create table temp.up_appd_&sampleBase3. as
	select t1.*, t2.fieldnm
	from up_appd t1 left join list t2 on (t1.varname=t2.dvar)
	order by t2.fieldnm, t1.round, t1.row, t1.ub;
quit; /*146,671*/ /*147,409*/ /*114,756*/ /*137,812*/ /*143,447*/
proc sql; create table temp.up_intm_&sampleBase3. as
	select t1.*, t2.fieldnm
	from up_intm t1 left join list t2 on (t1.varname=t2.dvar)
	order by t2.fieldnm, t1.row, t1.ub;
quit; /*26,291*/ /*26,133*/ /*26,133*/ /*27,380*/ /*28,169*/
proc sql; create table temp.up_fnl_&sampleBase3. as
	select t1.*, t2.fieldnm
	from up_fnl t1 left join list t2 on (t1.varname=t2.dvar)
	order by t2.fieldnm, t1.row, t1.ub;
quit; /*37,983*/ /*40,944*/ /*35,118*/ /*36,896*/ /*40,999*/ /*40,995(nonAOA exclude exempted)*/








*group by MAD output binning;
proc sql; create table bin_down as
	select fieldnm, row as bin, substr(varname, 2, length(varname)-1) as var, trend,
			sum(count_good) as good, sum(count_bad) as bad
	from temp.down_fnl_&sampleBase3._&version.
	group by fieldnm, bin, var, trend
	order by fieldnm, bin, trend;
quit; /*12,853*/ /*13,486*/ /*9,883*/ /*13,307*/ /*14,049*/ /*10,628*/ /*12,305*/ /*12,295(nonAOA exclude exempted)*/

proc sql; create table bin_up as
	select fieldnm, row as bin, substr(varname, 2, length(varname)-1) as var, trend,
			sum(count_good) as bad, sum(count_bad) as good				/*uptrend MAD exchange good & bad*/
	from temp.up_fnl_&sampleBase3._&version.
	group by fieldnm, bin, var, trend
	order by fieldnm, bin, trend;
quit; /*11,240*/ /*12,953*/ /*8,641*/ /*11,577*/ /*13,493*/ /*9,069*/ /*11,547*/ /*11,546(nonAOA exclude exempted)*/
















/******************************************* start calculate gini *******************************************/
/******************************************* start calculate gini *******************************************/
/******************************************* start calculate gini *******************************************/
/*factor listing*/
proc sql; create table list_down as
	select distinct fieldnm, var
	from bin_down
	order by fieldnm;
quit; /*3,163*/ /*3,355*/ /*3,370*/

/*defined global variable*/
proc sql noprint;
	select count(var) into: dscnt
	from list_down; 
 
	%let dscnt = %trim(&dscnt);
	select var into: dsname1-: dsname&dscnt
	from list_down;   
quit; 
%put &dsname5;








proc sql; create table bin_down as
	select *, sum(good) as total_good, sum(bad) as total_bad
	from bin_down
	group by fieldnm; 
quit; /*13,307*/ /*10,628*/ /*12,305*/ /*12,295(nonAOA exclude exempted)*/
*downtrend MAD GINI;
/**********************************		 Compute binning GINI		 **********************************/
data bin_temp; set bin_down /*(drop=ind)*/;
	if good = 0 and bad = 0 then do;
		BadRate = 0;
		GoodPct = 0;
		BadPct = 0;
	end; 
	else do;
		BadRate = Bad/sum(Good,Bad);
		GoodPct = Good/total_good;
		BadPct  = Bad/total_bad;
	end;
run; /*12,853*/ /*9,883*/ /*13,307*/ /*10,628*/ /*12,305*/ /*12,295(nonAOA exclude exempted)*/
/*sort by Bin to compute the binning GINI*/
proc sort data=bin_temp; by fieldnm descending Bin; run; 
%gini(bin_temp); /*3,163*/ /*3,355*/ /*3,370*/


/*import factor long list excel*/
proc import out=fac_format 
	datafile="C:\Users\115861\Desktop\MGA\07. working\005. factor_long_list_v3.xlsx"
	dbms=xlsx replace;
	sheet='Factor Selection';
run; /*3,163*/ /*3,355*/ /*3,370*/
/*summ left join factor long list excel*/
proc sql; create table Shortlisted_summ_abs_Down as 
	select 	a.var, a.VOI, a.GINI as GINI_abs, a.KS as KS_abs, a.BIN, 
			a.fieldnm, a.max_concentration, a.hit_rate, b.*
	from Shortlisted_summ as a left join fac_format as b on a.var=b.var
	order by b.number;
quit; /*3,163*/ /*3,355*/ /*3,370*/



proc sql; create table bin_up as
	select *, sum(good) as total_good, sum(bad) as total_bad
	from bin_up
	group by fieldnm; 
quit; /*11,577*/ /*9,069*/ /*11,546(nonAOA exclude exempted)*/
*uprend MAD GINI;
/**********************************		 Compute binning GINI		 **********************************/
data bin_temp; set bin_up /*(drop=ind)*/;
	if good = 0 and bad = 0 then do;
		BadRate = 0;
		GoodPct = 0;
		BadPct = 0;
	end; 
	else do;
		BadRate = Bad/sum(Good,Bad);
		GoodPct = Good/total_good;
		BadPct  = Bad/total_bad;
	end;
run; /*11,240*/ /*11,577*/ /*9,069*/ /*11,546(nonAOA exclude exempted)*/
/*sort by Bin to compute the binning GINI*/
proc sort data=bin_temp; by fieldnm descending Bin; run; 
%gini(bin_temp); /*3,355*/ /*3,370*/


/*import factor long list excel*/
proc import out=fac_format 
	datafile="C:\Users\115861\Desktop\MGA\07. working\005. factor_long_list_v3.xlsx"
	dbms=xlsx replace;
	sheet='Factor Selection';
run; /*3,355*/ /*3,370*/
/*summ left join factor long list excel*/
proc sql; create table Shortlisted_summ_abs_Up as 
	select a.var, a.VOI, a.GINI as GINI_abs, a.KS as KS_abs, a.BIN, a.fieldnm, a.max_concentration, a.hit_rate, b.*
	from Shortlisted_summ as a left join fac_format as b on a.var=b.var
	order by b.number;
quit; /*3,163*/ /*3,355*/ /*3,370*/









proc sql; create table noHitcnt as
	select varname, fieldnm, trend, sum(count_good) as sum_good, sum(count_bad) as sum_bad
	from temp.down_fnl_&sampleBase3._&version.
	where ub in ('00. Blank','01. -99999','02. -99998','03. -99997','04. -99996','05. -99995','06. -99994')
	group by varname, fieldnm, trend
	order by varname, fieldnm, trend;
quit; /*3,237*/ /*3,319*/
proc sql; create table Hitcnt as
	select varname, fieldnm, trend, sum(count_good) as sum_good, sum(count_bad) as sum_bad
	from temp.down_fnl_&sampleBase3._&version.
	where ub not in ('00. Blank','01. -99999','02. -99998','03. -99997','04. -99996','05. -99995','06. -99994')
	group by varname, fieldnm, trend
	order by varname, fieldnm, trend;
quit; /*3,365*/
proc sql; create table HitRateTable as
	select coalescec(t1.varname,t2.varname) as varname, coalescec(t1.fieldnm,t2.fieldnm) as fieldnm, coalescec(t1.trend,t2.trend) as trend,
			sum(t2.sum_good,t2.sum_bad) as sum_hit, sum(t2.sum_good,t2.sum_bad,t1.sum_good,t1.sum_bad) as sum_all,
			(calculated sum_hit)/(calculated sum_all) as Hit_rate
	from noHitcnt t1 full join Hitcnt t2 on (t1.fieldnm=t2.fieldnm)
	order by calculated fieldnm;
quit; /*3,370*/
/*******************	 To obtain one output for both GINI Absolute and GINI Ultimate	 ********************/
proc sql; create table Shortlisted_summ_f as 
	select	a.fieldnm, 
			a.GINI_abs 	as GINI_abs_down, 	a.KS_abs 	as KS_abs_down,		a.max_concentration as max_concentration_down,
		   	a.VOI 		as VOI_down,	  	a.bin		as bin_down,
		   	b.GINI_abs 	as GINI_abs_up, 	b.KS_abs 	as KS_abs_up,		b.max_concentration as max_concentration_up,
			b.VOI 		as VOI_up,	  		b.bin		as bin_up,
			t3.Hit_rate	as Hit_rate_default,
		   	a.var, a.number, a.factor, a.time_frame, a.exclusion, 
			a.group, a.category, a.sub_category, a.product_type, a.type, a.data_format, a.impute,
			"&sampleBase." as sampleBase
	from Shortlisted_summ_abs_Down a left join Shortlisted_summ_abs_Up b on a.var=b.var
/*								   	 left join outt.Shortlisted_summ_&versionHitRate._&sampleBase3. t3 on (a.var=t3.var)*/
									 left join HitRateTable											t3 on (a.fieldnm=t3.fieldnm)
	order by a.number;
quit; /*3,163*/ /*3,355*/ /*3,370*/
data Shortlisted_summ_f1; set Shortlisted_summ_f;
	GINI_abs_down	=abs(GINI_abs_down);
	GINI_abs_up		=abs(GINI_abs_up);
	GINI_abs_max	=max(abs(GINI_abs_down),abs(GINI_abs_up));

		 if GINI_abs_max = GINI_abs_down 	then trend_match='Down';
	else if GINI_abs_max = GINI_abs_up 		then trend_match='Up';

	length GINI_abs_down 8.;
	length GINI_abs_up 8.;
	length GINI_abs_max 8.;

		 if trend_match='Down' 				then KS_abs_match				=KS_abs_down;
	else if trend_match='Up' 				then KS_abs_match				=KS_abs_up;

		 if trend_match='Down' 				then max_concentration_match	=max_concentration_down;
	else if trend_match='Up' 				then max_concentration_match	=max_concentration_up;

		 if trend_match='Down' 				then VOI_match					=VOI_down;
	else if trend_match='Up' 				then VOI_match					=VOI_up;

		 if trend_match='Down' 				then bin_match					=bin_down;
	else if trend_match='Up' 				then bin_match					=bin_up;

	if GINI_abs_max > 10					then GINI_abs_max_gt10 	=1;
	else 						 				 GINI_abs_max_gt10 	=0;
	if max_concentration_match < 0.90		then concentration_lt90	=1;
	else										 concentration_lt90	=0;
	if hit_rate_default >= 0.3				then hitrate_ge30		=1;
	else										 hitrate_ge30		=0;

	if GINI_abs_max_gt10 =1 and concentration_lt90 =1 and hitrate_ge30 =1	then Pass_3=1; else	Pass_3=0;
run; /*3,163*/ /*3,355*/ /*3,370*/








/*output excel*/
/*data outt.Shortlisted_summ_&version._&sampleBase3.;*/
/*data outt.Shortlisted_summ_&version._&sampleBase3._addit2;*/
/*data outt.Shortlisted_summ_&version._&sampleBase3._additi;*/
data outt.Shortlisted_summ_&version._&sampleBase3.;
	retain 	number group category sub_category Product_Type	fieldnm	var	Factor	
			data_format Type Impute Time_Frame	Exclusion
			GINI_abs_down 			GINI_abs_up 			GINI_abs_max
			KS_abs_down  			KS_abs_up 				KS_abs_match
			VOI_down				VOI_up					VOI_match
			max_concentration_down	max_concentration_up 	max_concentration_match
			BIN_down				BIN_up					BIN_match
			trend_match
			Pass;
	set Shortlisted_summ_f1;
	sampleBase="&sampleBase";
run; /*3,163*/ /*3,355*/ /*3,370*/
data Shortlisted_summ_f3; set outt.Shortlisted_summ_&version._&sampleBase3.;
run;

PROC EXPORT DATA= Shortlisted_summ_f3
            OUTFILE= "C:\Users\115861\Desktop\MGA\07. working\008. Shortlisted_summ_&version._&sampleBase3..xlsx"
            DBMS=xlsx REPLACE;
			sheet="Shortlisted_summ";
RUN;





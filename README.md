# Test
测试项目
package com.bignerdranch.android.myandroid;

import android.annotation.SuppressLint;
import android.app.Activity;
import android.content.Intent;
import android.os.Bundle;
import android.util.Log;
import android.view.Menu;
import android.view.View;
import android.widget.Button;
import android.widget.ImageButton;
import android.widget.TextView;
import android.widget.Toast;

public class MyActivity extends Activity {
	private static final String TAG="MyActivity";
	private static final String KEY_INDEX="index";
	private Button mTrueButton;
	private Button mFalseButton;
	private ImageButton mNextButton;
	private TextView mQuestionTextView;
	private ImageButton mBeforeButton;
	private Button mCheatButton;
	
	private TrueFalse[] mQuestionBank = new TrueFalse[]{
		new TrueFalse(R.string.question_1,true),
		new TrueFalse(R.string.question_2,false),
		new TrueFalse(R.string.question_3,false),
		new TrueFalse(R.string.question_4,true),
		new TrueFalse(R.string.question_5,true),
	};
	private int mCurrentIndex=0;
	private boolean mIsCheater;
	private void updateQuestion(){
		int question=mQuestionBank[mCurrentIndex].getQuestion();
		mQuestionTextView.setText(question);	
	}
	private void checkAnswer(boolean userPressedTrue){
		boolean answerIsTrue=mQuestionBank[mCurrentIndex].isTrueQuestion();
		int messageResId=0;
		if(mIsCheater){
			messageResId=R.string.judgment_button;
		}else{
			
		if(userPressedTrue==answerIsTrue){
			messageResId=R.string.correct_toast;
		}else{
			messageResId=R.string.incorrcet_toast;
		}
		}
		Toast.makeText(this,messageResId, Toast.LENGTH_SHORT).show();
	}
    @SuppressLint("CutPasteId") @Override
    protected void onActivityResult(int requestCode,int resultCode,Intent data){
    	if(data==null){
    		return;
    	}
    	mIsCheater=data.getBooleanExtra(CheatActivity.EXTRA_ANSWER_SHOWN, false);
    }
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        Log.d(TAG, "onCreate(Bundle)called");
        setContentView(R.layout.activity_my);
        mQuestionTextView=(TextView)findViewById(R.id.text1);
        
        mTrueButton=(Button)findViewById(R.id.true_button);
        mTrueButton.setOnClickListener(new View.OnClickListener() {
			
			@Override
			public void onClick(View v) {
				checkAnswer(true);
			}
			});
        mFalseButton=(Button)findViewById(R.id.false_button);
        mFalseButton.setOnClickListener(new View.OnClickListener() {
			
			@Override
			public void onClick(View v) {
				checkAnswer(false);
			}
		});
        mNextButton=(ImageButton)findViewById(R.id.next_button);
        mNextButton.setOnClickListener(new View.OnClickListener() {
			
			@Override
			public void onClick(View v) {
				mCurrentIndex=(mCurrentIndex+1)%mQuestionBank.length;
				mIsCheater=false;
				updateQuestion();
			}
		});
        mBeforeButton=(ImageButton)findViewById(R.id.before_button);
        mBeforeButton.setOnClickListener(new View.OnClickListener() {
			
			@Override
		public void onClick(View v) {
				mCurrentIndex=(mCurrentIndex-1)%mQuestionBank.length;
				mIsCheater=false;
				updateQuestion();
			}
		});
        mCheatButton=(Button)findViewById(R.id.cheat_button);
        mCheatButton.setOnClickListener(new View.OnClickListener() {
			
			@Override
			public void onClick(View v){
				Intent i=new Intent(MyActivity.this,CheatActivity.class);
				boolean answerIsTrue=mQuestionBank[mCurrentIndex].isTrueQuestion();
				i.putExtra(CheatActivity.EXTRA_ANSWER_IS_TRUE, answerIsTrue);
				startActivityForResult(i,0);
			}
		});
        if(savedInstanceState!=null){
        	mCurrentIndex=savedInstanceState.getInt(KEY_INDEX,0);
        }
        updateQuestion();
    }
    @Override
	public void onSaveInstanceState(Bundle savedInstanceState){
    	super.onSaveInstanceState(savedInstanceState);
    	Log.i(TAG,"onSaveInstanceState");
    	savedInstanceState.putInt(KEY_INDEX, mCurrentIndex);
    }
    public void onStart(){
    	super.onStart();
    	Log.d(TAG, "onStart()called");
    }
    public void onPause(){
    	super.onPause();
    	Log.d(TAG, "onPause()called");
    }
    public void onResume(){
    	super.onResume();
    	Log.d(TAG, "onResume()called");
    }
    public void onStop(){
    	super.onStop();
    	Log.d(TAG, "onStop()called");
    }
    public void onDestroy(){
    	super.onDestroy();
    	Log.d(TAG, "onDestroy()called");
    }
    public boolean onCreateOptionsMenu(Menu menu) {
        // Inflate the menu; this adds items to the action bar if it is present.
        getMenuInflater().inflate(R.menu.my, menu);
        return true;
    }
   
}

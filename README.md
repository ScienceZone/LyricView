# LyricView
�����ֲ�������Ŀ��ʹ�õ���ʾ��ʵĹ���,ʵ���ò�ͬ��ɫ�������������ڲ��ź�δ���Ÿ��
��ͼ����(��ͼЧ�����Ǻܺ�,������Demo�鿴):

![]{/ScreenShot.gif}

ʹ�÷�ʽ��
	<com.zdc.lyricdemo.view.LyricView
			android:id="@+id/lvView"
			android:layout_width="match_parent"
			android:layout_height="420dp"
			android:background="@drawable/base_bg" />
Ȼ���ڴ����г�ʼ��MediaPlayer����ʡ����ü�����
	public class TtActivity extends Activity implements OnClickListener {

		private Button btnPlayPause;
		private LyricView lvView;
		private MediaPlayer mediaPlayer = new MediaPlayer();
		private Handler handler = new Handler();

		@Override
		protected void onCreate(Bundle savedInstanceState) {
			super.onCreate(savedInstanceState);
			setContentView(R.layout.activity_main);
			initView();
			initMediaPlayer();
		}

		private void initView() {
			btnPlayPause = (Button) findViewById(R.id.btn_play_pause);
			lvView = (LyricView) findViewById(R.id.lvView);
			btnPlayPause.setOnClickListener(this);
		}

		private void initMediaPlayer() {
			AssetManager am = getAssets();
			try {
				mediaPlayer.reset();
				mediaPlayer.setDataSource(am.openFd("cbg.mp3").getFileDescriptor());
				mediaPlayer.prepare();
			} catch (IOException e) {
				e.printStackTrace();
			}
			lvView.initLyricFile(LyricLoader.loadLyricFile("cbg"));
		}

		@Override
		public void onClick(View view) {
			if (!mediaPlayer.isPlaying()) {
				mediaPlayer.start();
				handler.post(runnable);
			} else {
				mediaPlayer.pause();
				handler.removeCallbacks(runnable);
			}
		}

		private Runnable runnable = new Runnable() {
			@Override
			public void run() {
				if (mediaPlayer.isPlaying()) {
					long time = mediaPlayer.getCurrentPosition();
					lvView.updateLyrics((int) time, mediaPlayer.getDuration());
				}
				handler.postDelayed(this, 100);
			}
		};

		@Override
		protected void onDestroy() {
			handler.removeCallbacks(runnable);
			mediaPlayer.reset();
			mediaPlayer.release();
			mediaPlayer = null;
			super.onDestroy();
		}
	}	